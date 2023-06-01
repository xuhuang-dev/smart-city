<template>
	<view>
		<view class="">
			<u-cell-group>
				<!-- <u-cell-item title="头像" :arrow="false">
					<u-avatar :src="baseUrl+info.avatar" @click="chooseImage"  size="140"></u-avatar>
				</u-cell-item> -->
				<u-cell-item  title="用户名" :arrow="false">
					<input type="text" v-model="info.userName" disabled />
				</u-cell-item>
				<u-cell-item  title="昵称" :arrow="false">
					<input type="text" v-model="info.nickName"  />
				</u-cell-item>
				<u-cell-item  title="邮箱" :arrow="false">
					<input type="email" v-model="info.email"  />
				</u-cell-item>
				<u-cell-item  title="证件号" :arrow="false">
					<view @click="phModal=true" style="font-size: 16px;">{{idCard}}</view>
				</u-cell-item>
				<u-cell-item  title="手机号" :arrow="false">
					<view @click="isModal=true" style="font-size: 16px;">{{phonenumber}}</view>
				</u-cell-item>
				<u-cell-item  title="性别" :arrow="false">
					<u-radio-group v-model="info.sex">
					<u-radio name="0">男</u-radio>
					<u-radio name="1">女</u-radio>
					</u-radio-group>
				</u-cell-item>
			</u-cell-group>
		</view>
		<u-modal v-model="isModal">
			<u-cell-item  title="手机号" :arrow="false">
				<input type="number" v-model="info.phonenumber" @blur="phonenumberChange"/>
			</u-cell-item>
		</u-modal>
		<u-modal v-model="phModal">
			<u-cell-item  title="证件号" :arrow="false">
				<input type="idcard" v-model="info.idCard" @blur="idCardChange"/>
			</u-cell-item>
		</u-modal>
		
		<u-button @click="save" >修改</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phModal:false,
				isModal:false,
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					baseUrl:"",
					info:{},
					idCard:"",
					phonenumber:"",
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
			},
			onShow(){
				this.$u.get("/prod-api/api/common/user/getInfo").then(res=>{
					this.info = res.user
					this.phonenumber = this.info.phonenumber.substr(0,7) + '****'
					this.idCard = this.info.idCard.substr(0,4)+"********"+this.info.idCard.substr(-4,4)
				})
			},
		// watch: {
		// 	info.phonenumber() {
		// 		return this.info.phonenumber.substr(0,7) + '****';
		// 	}
		// },
		methods: {
			idCardChange(){
				this.idCard = this.info.idCard.substr(0,4)+"********"+this.info.idCard.substr(-4,4)
			},
			phonenumberChange() {
				this.phonenumber = this.info.phonenumber.substr(0,7) + '****'
			},
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
				this.$u.put("/prod-api/api/common/user",this.info).then(res=>{
					this.$u.toast("修改成功")
				})
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
