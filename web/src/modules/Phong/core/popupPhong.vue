<template>
  <v-dialog :value="value" max-width="1000px" persistent >
    <form >
      
    </form>
  <v-card>         
    <v-card-text>
      <v-container fluid grid-list-md>
        <v-layout justify-center>
          <h1 class="mb-2 danhsachPT">Thông tin phòng trọ</h1>    
        </v-layout>     
        <v-form  ref="form" v-model="valid" lazy-validation>
          <v-layout row wrap justify-space-between>
            <v-flex xs12 sm6 md6>
              <v-text-field v-model="formData.tenPhong"  :rules="tenPhongRules" :counter="20" label="Tên Phòng Trọ"></v-text-field>
            </v-flex>   
            <v-flex xs12 sm6 md6 style="display: inline-flex;">
              <div style="width:80%;">
                <v-select
                  v-model="formData.khuPhongID"
                  :items="dsKhuPhong"
                  :rules="khuPhongRules"
                  required
                  label="Khu Phòng"
                  item-text="tenKhuPhong"
                  item-value="_id"
                ></v-select>
              </div>
              <div style="width:20%">
                <v-btn @click="openKhuPhong=true"  style="width:100%; margin-top:20px;"  flat  color="blue"><v-icon left>add</v-icon></v-btn>
              </div>
            </v-flex>   
            <v-flex xs12 sm6 md6>
              <v-select
                v-model="formData.tinhTrangPhongID"
                :items="dsTinhTrangPhong"
                label="Tình Trạng Phòng"
                :rules="tinhTrangPhongRules"
                required
                item-text="tenTinhTrangPhong"
                item-value="_id"
              ></v-select>
            </v-flex>   
            <v-flex xs12 sm6 md6 style="display: inline-flex;">
               <div style="width:80%;">
                  <v-select
                    v-model="formData.loaiPhongID"
                    :items="dsLoaiPhong"
                    :rules="loaiPhongRules"
                    required
                    @change="getPhongInput"
                    label="Loại Phòng"
                    item-text="tenLoaiPhong"
                    item-value="_id"
                  ></v-select>
              </div>
              <div style="width:20%">
                <v-btn @click="openLoaiPhong=true"  style="width:100%; margin-top:20px;"  flat  color="blue"><v-icon left>add</v-icon></v-btn>
              </div>
            </v-flex>   
          </v-layout>
          <v-layout row wrap justify-space-between>   
            <v-flex shrink  size="10px">
              <div>
                <input hidden type="file"   style="display: none" ref="file" accept="image/*"  @change="choosedFile">
                <v-btn  color="success"  @click="chooseFIle">Ảnh Chính</v-btn>
              </div>
              <div v-if="isThem">
                  <img :src="anhChinh"  height="144">
              </div>
              <div v-else> 
                <div v-if="srcAnhChinh">
                  <img :src="srcAnhChinh" height="144">
                </div>                            
              </div>
            </v-flex>
            <v-flex grow size="10px">
              <div>
                <input hidden type="file" style="display: none" ref="multifile" multiple accept="image/*"  @change="choosedFileMulti">
                <v-btn color="success" @click="chooseFIleMulti">Ảnh Chi Tiết</v-btn>
              </div>
              <div v-if="isThem">
                <img v-for="(item,index) in anhChiTiet" :key="index" style="padding-right:5px;" :src="item" height="144">
              </div>
              <div v-else>
                 <div v-if="srcAnhChiTiet">
                   <span v-for="(item,index) in srcAnhChiTiet" :key="index">
                     <img v-if="item.type === 'base64'" style="padding-right:5px;" :src="item.anh" height="144">
                     <img v-if="item.type === 'src'"  style="padding-right:5px;" :src="'//localhost:3003/image/' + item.anh" height="144">
                   </span>
                 </div>        
              </div>
            </v-flex>       
          </v-layout>
          <v-layout row wrap>
            <v-flex xs12 sm6 md4>
              <v-textarea
                height="70"
                outline
                :rules="moTaRules"
                label="mô tả phòng trọ"
                v-model="formData.moTa"
                required
              ></v-textarea>
            </v-flex>                                                                                     
            <v-flex xs6 sm6 md2>
                <v-text-field
                  height="70"
                  type="number"
                  label="Số điện"
                  hide-details
                  v-model="formData.soDien"
                  outline
                ></v-text-field>
            </v-flex>                                                                                     
            <v-flex xs6 sm6 md2>
                <v-text-field
                  height="70" 
                  type="number"
                  label="Số nước"
                  hide-details
                  v-model="formData.soNuoc"
                  outline
                ></v-text-field>
            </v-flex> 
            <v-flex xs12 sm6 md4>
              <v-text-field
                height="70"
                type="number"
                label="Giá phòng"
                hide-details
                v-model="formData.giaPhong"
                outline
              ></v-text-field>
            </v-flex>                                                                                        
          </v-layout>
          <v-layout row wrap justify-center>
            <v-flex xs6 sm6 md2>
              <v-checkbox
                v-model="formData.dKMang"
                label="Đăng ký mạng"
                color="success"
                :value="false"
                hide-details
              ></v-checkbox>
            </v-flex>
            <v-flex xs6 sm6 md2 shrink>
              <v-checkbox
                v-model="formData.homeFlag"
                label="Đăng trang chủ"
                color="success"
                :value="false"
                hide-details
              ></v-checkbox>
            </v-flex>
            <v-flex xs6 sm6 md2>
              <v-checkbox
                v-model="formData.status"
                label="Kích hoạt"
                color="success"
                :value="false"
                hide-details
              ></v-checkbox>
            </v-flex>
            <v-flex xs6 sm6 md2 >
              <v-checkbox
                v-model="formData.hotFlag"
                label="Phòng hot"
                color="success"
                :value="false"
                hide-details
              ></v-checkbox>
            </v-flex>
          </v-layout>                                                                          
        </v-form>                                                       
        <v-layout row wrap mt-2>
            <v-flex xs12 sm12 md12 style="display: flex !important;justify-content: center !important;">                   
              <v-btn color="blue darken-1"  dark @click="XacNhan" >Xác nhận</v-btn>
              <v-btn color="yellow darken-1" dark @click="Huy" >Hủy</v-btn>
            </v-flex>
        </v-layout>  
      </v-container>
    </v-card-text>          
  </v-card>
  <khuPhong v-model="openKhuPhong"/>
  <loaiPhong v-model="openLoaiPhong"/>
</v-dialog>
</template>

<script src="./popupPhong.js">
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
.danhsachPT {
    border-bottom: 4px solid tomato;  
}
</style>


