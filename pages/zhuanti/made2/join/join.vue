<template>
	<view>
		<view class="">
			<view class="title-lg u-text-center" style="background-color: white;">入驻申请信息表</view>
			<u-cell-group>
				<!-- <u-cell-item @click="chooseImage" title="头像" :arrow="false">
					<u-avatar :src="baseUrl+info.avatar" size="140"></u-avatar>
				</u-cell-item> -->
				<u-cell-item  title="名称" :arrow="false">
					<input type="text" v-model="info.userName"  />
				</u-cell-item>
				<u-cell-item  title="地址" :arrow="false">
					<input type="text" v-model="info.nickName"  />
				</u-cell-item>
				<u-cell-item  title="邮箱" :arrow="false">
					<input type="email" v-model="info.email"  />
				</u-cell-item>
				
				<u-cell-item  title="手机号" :arrow="false">
					<input type="text" v-model="info.phonenumber"  />
				</u-cell-item>
				<!-- <u-cell-item  title="性别" :arrow="false">
					<u-radio-group v-model="info.sex">
					<u-radio name="0">男</u-radio>
					<u-radio name="1">女</u-radio>
						
					</u-radio-group>
				</u-cell-item> -->
			</u-cell-group>
		</view>
		<u-button @click="save" >提交</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					username:"",
					password:"",
					baseUrl:"",
					info:{},
					idCard:"",
					idCard:"",
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
			},
			onShow(){
				// this.$u.get("/prod-api/api/common/user/getInfo").then(res=>{
				// 	this.info = {...res.user}
				// 	console.log(this.info)
				// 	if(this.info.idCard!=null){
				// 		this.info.idCard = this.info.idCard.substr(0,4)+"********"+this.info.idCard.substr(-4,4)
				// 	}else this.info.idCard = ""
				// 	if(this.info.phonenumber!=null){
				// 		this.info.phonenumber = this.info.phonenumber.substr(0,7)+"****"
				// 	}else this.info.phonenumber = ""
				// })
			},
		methods: {
			chooseImage(){
				uni.chooseImage({
					count:1,
					success: (res) => {
						this.info.avatar = res.tempFilePaths[0]
						this.uploadFile()
					}
				})
			},
			uploadFile(){
				uni.uploadFile({
					url:this.baseUrl+"/prod-api/common/upload",
					filePath:this.info.avatar,
					header:{
						"authorization":uni.getStorageSync("token")
					},
					success:(res)=>{
						res = JSON.parse(res.data)
						this.info.avatar = "/prod-api"+res.fileName
					}
				})
			},
			save(){
				// this.$u.put("/prod-api/api/common/user",this.info).then(res=>{
				// 	this.$u.toast("修改成功")
				// })
					this.$u.toast("提交成功")
			}
		}
	}
</script>

<style lang="scss">
page{
	background-color: #ededed;
}

.camera{
	width: 54px;
	height: 44px;
	
	&:active{
		background-color: #ededed;
	}
}
.user-box{
	background-color: #fff;
}
</style>
