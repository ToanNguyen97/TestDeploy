<template>
<div style="background-color:#fff; height:100vh">
  <div v-if="booking" >
    <div class="col-md-6  offset-md-3 content" style="margin-top: 60px;">
      <svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 130.2 130.2">
        <circle class="path circle" fill="none" stroke="#73AF55" stroke-width="6" stroke-miterlimit="10" cx="65.1" cy="65.1" r="62.1"/>
        <polyline class="path check" fill="none" stroke="#73AF55" stroke-width="6" stroke-linecap="round" stroke-miterlimit="10" points="100.2,40.2 51.5,88.8 29.8,67.5 "/>
      </svg>
      <v-flex style="display:flex;justify-content:center">
        <h2 class="thank-you">Kích hoạt thành công<br>hẹn gặp lại bạn vào ngày {{booking.ngayNhanPhong | formatDate}}!</h2>
      </v-flex>
			<div class="box" v-if="booking && phongInfo">
				<h1>Thông tin đơn đặt phòng</h1>
				<p style="text-align:start;" >
					Họ Tên: {{booking.hoNguoiBook.trim()}} {{booking.tenNguoiBook}}<br>
					Số Điện Thoại: {{booking.soDienThoai}}<br>
					Địa Chỉ: {{booking.diaChi}}<br>
					Tên Phòng: {{phongInfo.tenPhong}}, khu Phòng: <span v-if="phongInfo && phongInfo.khuPhongID ">{{phongInfo.khuPhongID.tenKhuPhong}}</span><br>
					Giá Phòng: {{phongInfo.giaPhong | formatCurrency}}<br>
					Loại Phòng: <span v-if="phongInfo && phongInfo.loaiPhongID">{{phongInfo.loaiPhongID.tenLoaiPhong}}</span>
				</p>
			</div>
      <a class="btn" href="/index.html">BACK TO HOME</a>
    </div>
  </div>
</div>
</template>
<script>
import moment from 'moment'
import axios from 'axios'
export default {
  data() {
    return {
			loading: false,
			booking: {},
			phongInfo: {}
    }
  },
  created() {
    this.loading = true
    axios.get(`${window.urlApi}api/kich-hoat-phong-${this.$route.params.id}`).then( res => {
			this.loading = true
			this.booking = res.data
			this.phongInfo = this.booking.phongID
    }).catch(err => {
      console.log(err)
    })
	},
	filters: {
		formatDate (date) {
			return moment(date).format('DD/MM/YYYY')
		},
		formatCurrency (money) {
			if(money) {
				return money.toLocaleString('it-IT', {style : 'currency', currency : 'VND'})
			}
			return 0
		}
	}
}
</script>
<style scoped>
.box {
	width: 500px;
	height: auto;
	margin: 5px auto;
	border: 1px dashed blue;
}
.box>p{
	margin-left:5px;
}
.btn{
    background-color: #4fc3f7;
    position: relative;
    display: inline-block;
    width: 358px;
    padding: 5px;
    z-index: 5;
    font-size: 25px;
    margin:0 auto;
    color:#fff;
    text-decoration: none;
    margin-right: 10px
}
	.thank-you{
		border-bottom: 1px solid #ddd;
    padding: 10px;
    text-align:center;
    width:500px;
	}
	.center {
		display: block;
		margin-left: auto;
		margin-right: auto;
	}
	.content{
		font-size: 16px;
		text-align: center;
	}
	#share {
		text-align: center;
	}
	.trading-code code{
		font-size: 22px;
		font-weight: bold;
	}
	/* colors */
	svg {
		width: 100px;
		display: block;
		margin: 40px auto 0;
	}
	.path {
		stroke-dasharray: 1000;
		stroke-dashoffset: 0;
	}
	.path.circle {
		-webkit-animation: dash 0.9s ease-in-out;
		animation: dash 0.9s ease-in-out;
	}
	.path.line {
		stroke-dashoffset: 1000;
		-webkit-animation: dash 0.9s 0.35s ease-in-out forwards;
		animation: dash 0.9s 0.35s ease-in-out forwards;
	}
	.path.check {
		stroke-dashoffset: -100;
		-webkit-animation: dash-check 0.9s 0.35s ease-in-out forwards;
		animation: dash-check 0.9s 0.35s ease-in-out forwards;
	}
	p {
		text-align: center;
		margin: 20px 0 30px;
		font-size: 1.25em;
	}
	p.success {
		color: #73AF55;
	}
	p.error {
		color: #D06079;
	}
	@-webkit-keyframes dash {
		0% {
			stroke-dashoffset: 1000;
		}
		100% {
			stroke-dashoffset: 0;
		}
	}
	@keyframes dash {
		0% {
			stroke-dashoffset: 1000;
		}
		100% {
			stroke-dashoffset: 0;
		}
	}
	@-webkit-keyframes dash-check {
		0% {
			stroke-dashoffset: -100;
		}
		100% {
			stroke-dashoffset: 900;
		}
	}
	@keyframes dash-check {
		0% {
			stroke-dashoffset: -100;
		}
		100% {
			stroke-dashoffset: 900;
		}
	}
</style>