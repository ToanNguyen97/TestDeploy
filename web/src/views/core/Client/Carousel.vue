<template>
  <div >
    <v-img  style="height:100vh;" :src="require('@/assets/banner2.jpg')"
      :lazy-src="require('@/assets/banner-lazy.jpg')"
      gradient="to right top,rgba(17, 17, 17, 0.95), rgba(244, 249, 250, 0)"
      class="yellow mb-2"
      >
      <template v-slot:placeholder>
        <v-layout fill-height align-center justify-center ma-0 >
          <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
        </v-layout>
      </template>
      <MenuTop/>
      <v-layout row  style="height:400px; padding-top:150px;" >
        <v-flex md7 class="mt-5 align-center justify-center" >
          <div class="textfomat">
            <span class="textbox display-1 font-weight-bold">Phòng trọ sinh viên Cô Mai</span>
          </div>
          <div class="textfomat pt-4">
            <span class="heading font-weight-bold">02 tổ 24 Nguyễn Đình Chiểu, phường Vĩnh Thọ, Nha Trang, Khánh Hòa</span>
          </div>
          <div class="textfomat pt-5">
            <span class="mr-2 textbox display-1 font-weight-bold">An ninh tuyệt đối</span>
            <span class="textbox display-1 font-weight-bold">Thoải mái, thân thiện</span>
          </div>
          <div class="textfomat pt-4">
            <span class="mr-2 textbox display-1 font-weight-bold">Giá cả hợp lý</span>
            <span class="mr-2 textbox display-1 font-weight-bold">Liên hệ: 0348096333</span> 
          </div>
          <div class="textfomat pt-5">
            <v-btn class="buttonFb" fab large flat  color="blue"><v-icon color="white" x-large>fab fa-facebook-square</v-icon></v-btn>
            <v-btn class="buttonContact" fab large flat  color="blue"><v-icon color="red" x-large>fab fa-google</v-icon></v-btn>
            <v-btn class="buttonContact" fab large flat  color="blue"><v-icon color="blue" x-large>fab fa-twitter</v-icon></v-btn>
          </div>
        </v-flex>
        <v-flex md5>
          <div class="boxCarousel" >
            <h1 class="texttimkiem"><v-icon large color="white" class="pr-2">search</v-icon>Tìm kiếm phòng</h1>
            <div class="mt-3">
              <v-select label="Chọn loại phòng" dark v-model="formData.loaiPhong" :items="dsLoaiPhong" item-text="tenLoaiPhong" item-value="_id" outline class="pa-2 labelSelect" hide-details></v-select>
              <v-select label="Chọn giá phòng" dark v-model="formData.giaPhong" :items="dsLoaiPhong" item-text="giaPhong" item-value="_id" outline class="px-2" hide-details></v-select>
              <div v-if="dsTinhTrangPhong" >
                <v-checkbox v-for="item of dsTinhTrangPhong" :key="item._id" class="pl-3"
                  style="display:inline-block"
                  dark
                  v-model="formData.tinhTrangPhongSelect"
                  :label="item.tenTinhTrangPhong"
                  :value="item._id"
                  color="orange"
                  hide-details
                ></v-checkbox>
                <v-checkbox class="pl-2"
                style="display:inline-block"
                  v-model="formData.isMang"
                  dark
                  label="Có dịch vụ mạng"
                  id="checkbox-oghep"
                  color="red"
                  hide-details
                ></v-checkbox>
              </div>
              <div style="text-align:center" class="pt-3">
                <v-btn color="cyan" @click="TimKiem" class="btnTimKiem" dark>Tìm Kiếm  <v-icon>search</v-icon> </v-btn>
              </div>
            </div>
          </div>
        </v-flex>
      </v-layout>
    </v-img>
    <div style="width:300px;height:10px;background:orange;margin:0 auto;"></div>
  </div>
</template>

<script src="./Carousel.js"></script>

<style scoped >
.boxCarousel {
  width: 430px;
  height:  380px;
  position: absolute;
  color: black;
  margin:30px 0 0 20px;
  border-radius: 12px;
  border: 1px solid #fff;
}
.textfomat {
  text-align: center;
  color: #fff;
  border-radius: 12px;
}
.textbox {
  padding: 0 10px 0 10px;
  border: 1px solid #fff;
  border-radius: 12px;
}
.buttonContact:hover {
  background: white;
}
.buttonFb:hover {
  background: rgb(85, 87, 243);
}
.texttimkiem {
  color: #fff;
  border-bottom: 1px solid orange;
}
</style>