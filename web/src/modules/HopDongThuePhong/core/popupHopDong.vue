<template>
  <v-dialog :value="value" max-width="1500px" persistent >
  <v-card>
    <v-form  ref="form" v-model="valid" lazy-validation>     
      <v-card-text>
        <v-container fluid grid-list-md>
          <v-layout  justify-center>
            <h1 class="mb-2 danhsachHD">Thông tin Hợp Đồng</h1>    
          </v-layout> 
          <v-layout row wrap>
            <v-flex xs12 sm12 md4>
              <v-card>
                <v-card-title class="pa-0">          
                  <v-layout row wrap style="padding:2px;" justify-space-between>
                    <v-flex xs6 sm6 md5>
                      <h3 class="pl-1 pt-2">Thông tin khách thuê</h3>
                    </v-flex>
                    <v-flex md2>
                      <v-btn icon @click="openKhach=true" dark color="pink"><v-icon>add</v-icon></v-btn>
                    </v-flex>             
                    <v-flex xs6 sm6 md5 pt-0 pb-2 pr-2>
                      <v-text-field type="number" style="padding-top:4px" v-model="soDienThoai" required :rules="soDienThoaiRules" :counter="13" single-line   label="nhập số điện thoại" append-icon="phone"></v-text-field>  
                    </v-flex>  
                    </v-layout>
                </v-card-title>
                <hr/>
                <v-card-text >
                  <v-layout row wrap>
                    <v-flex xs12 sm6 md3>
                      <v-avatar size="100" tile  v-if="khachThue" >
                        <img height="144" :src='`//localhost:3003/image/${khachThue.anhDaiDien}`' >
                      </v-avatar>
                      <v-avatar size="100" tile  v-else  >                     
                        <img height="144" src='//localhost:3003/image/avatar.png' alt="ảnh đại diện">
                      </v-avatar>
                      <v-btn depressed class="ml-1" :loading="loaKhach" @click="infoKhachThue" color="info">Xem</v-btn>
                    </v-flex>
                    <v-flex xs12 sm6 md9 v-if="khachThue">
                      <v-text-field style="padding-bottom:4px" :value="`${khachThue.hoKhachThue.trim()} ${khachThue.tenKhachThue}`" readonly  hide-details label="Họ tên" append-icon="fas fa-user"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly :value="khachThue.soCMND" hide-details label="Số chứng minh" append-icon="fas fa-address-card"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly hide-details :value="khachThue.gioiTinh? 'Nam': 'Nữ'" label="Giới tính" append-icon="fas fa-transgender"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly hide-details label="Địa chỉ" :value="khachThue.diaChi" append-icon="fas fa-map-marker-alt"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly hide-details label="Email" :value="khachThue.email" append-icon="mail"></v-text-field>
                    </v-flex>
                  </v-layout>
                  </v-card-text>
              </v-card>
            </v-flex>
            <v-flex xs12 sm12 md4>
              <v-card>
                <v-card-title class="pa-0" >
                  <v-layout row wrap style="padding:4px;">
                    <v-flex xs6 sm6 md4>
                      <h3 class="pl-1 pt-2" >Thông tin phòng</h3> 
                    </v-flex>             
                    <v-flex xs6 sm6 md4 pt-0 pb-2 pr-2>
                      <v-select 
                        class="pt-0"
                        required :rules="khuPhongIDRules"
                        single-line
                        v-model="khuPhongID"
                        :items="dsKhuPhong"
                        label="Chọn khu phòng"
                        item-text="tenKhuPhong"
                        item-value="_id"
                        @change="getPhong(khuPhongID)"
                      ></v-select>  
                    </v-flex>  
                    <v-flex xs6 sm6 md4 pt-0 pb-2 pr-2>
                      <v-select v-if="dsPhong"
                        class="pt-0"
                        hide-details
                        single-line
                        v-model="phongID"
                        :items="dsPhong"
                        label="chọn phòng"
                        item-text="tenPhong"
                        item-value="_id"
                      ></v-select>  
                    </v-flex>  
                    </v-layout>               
                </v-card-title>
                <hr/>
                <v-card-text >
                  <v-layout row wrap>
                    <v-flex xs12 sm6 md3>
                    <v-avatar size="100" tile  v-if="phong" >
                        <img height="144" :src='`//localhost:3003/image/${phong.anhChinh}`' >
                      </v-avatar>
                      <v-avatar size="100" tile  v-else  >                     
                        <img height="144" src='//localhost:3003/image/homebg.png' alt="ảnh phòng">
                      </v-avatar>
                      <v-btn depressed class="ml-1" :loading="loadPhong" @click="infoPhong" color="info">Xem</v-btn>
                    </v-flex>
                    <v-flex xs12 sm6 md9 v-if="phong">
                      <v-text-field style="padding-bottom:4px" readonly :value="phong.loaiPhongID.tenLoaiPhong"  hide-details label="Loại phòng" append-icon="fas fa-home"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly :value="phong.giaPhong"  hide-details label="Giá phòng" append-icon="fas fa-dollar-sign"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly hide-details :value="phong.tinhTrangPhongID.tenTinhTrangPhong" label="Tình trạng phòng" append-icon="fas fa-door-open"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly hide-details label="Số điện" :value="phong.soDien" append-icon="fas fa-bolt"></v-text-field>
                      <v-text-field style="padding-bottom:4px" readonly hide-details label="Số nước" :value="phong.soNuoc" append-icon="fas fa-tint"></v-text-field>
                    </v-flex>
                  </v-layout>
                  </v-card-text>
              </v-card>
            </v-flex>
            <v-flex xs12 sm12 md4>
              <v-card>
                <v-card-title class="pa-0" >
                  <v-layout row wrap style="padding:6px;"  justify-space-between>
                    <v-flex xs6 sm6 md6 >
                      <h3 class="pl-1 pt-2" >Thông tin hợp đồng</h3> 
                    </v-flex>               
                    </v-layout>
                </v-card-title>
                <hr/>
                <v-card-text >
                  <v-layout row wrap>
                    <v-flex xs12 sm6 md12>
                      <v-text-field style="padding-bottom:4px"  readonly v-model="soHD"  hide-details label="Số hợp đồng" append-icon="fas fa-address-book"></v-text-field>
                      <v-text-field style="padding-bottom:4px" v-if="khachThue" readonly :value="`${khachThue.hoKhachThue.trim()} ${khachThue.tenKhachThue}`"   hide-details label="tên khách" append-icon="fas fa-user"></v-text-field>
                      <v-text-field style="padding-bottom:4px" v-if="phong" :value="`${phong.tenPhong}`" readonly hide-details label="tên phòng" append-icon="fas fa-home"></v-text-field>
                      <v-menu v-model="menu" style="padding-bottom:9px" :close-on-content-click="false"  full-width  max-width="290"  >
                        <template v-slot:activator="{ on }">
                          <v-text-field
                            :value="formatNgayLap"
                            clearable
                            hide-details
                            label="Ngày Lập"
                            readonly
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-date-picker
                          locale=vi
                          v-model="formData.ngayLap"
                          @change="menu = false"
                        ></v-date-picker>
                      </v-menu> 
                      <v-menu v-model="menu1" :close-on-content-click="false"  full-width  max-width="290"  >
                      <template v-slot:activator="{ on }">
                        <v-text-field
                          :value="formatngayKetThuc"
                          clearable
                          :rules="ngayKetThucRules"
                          required
                          label="Ngày Kết Thúc"
                          readonly
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        locale=vi
                        v-model="formData.ngayKetThuc"
                        @change="menu1 = false"
                      ></v-date-picker>
                    </v-menu>
                    </v-flex>
                  </v-layout>
                  </v-card-text>
              </v-card>
            </v-flex>
          </v-layout>                                                          
          <v-layout row wrap mt-2>
              <v-flex xs12 sm12 md12 style="display: flex !important;justify-content: center !important;">                   
                <v-btn depressed color="blue darken-1" dark @click="XacNhan" >Xác nhận</v-btn>
                <v-btn depressed color="yellow darken-1" dark @click="Huy" >Hủy</v-btn>
              </v-flex>
          </v-layout> 
        </v-container>
      </v-card-text>
    </v-form>        
  </v-card>
  <popup-khach v-model="openKhach" :isThem="true"></popup-khach>
</v-dialog>
</template>

<script src="./popupHopDong.js">
</script>

<style >
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.colorText {
  color: #fff !important;
}
.danhsachHD {
    border-bottom: 4px solid tomato;  
}
.titleinfo {
  border-bottom: 2px solid  tomato
}
 .v-chip.true {
   background: blue
 }
 .v-chip.false {
  background: pink
 }

</style>


