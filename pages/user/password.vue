<template>
	<view>
		<view class="">
			<u-cell-group>
				<u-cell-item  title="旧密码" :arrow="false">
					<input type="password" v-model="oldPassword"  />
				</u-cell-item>
				<u-cell-item  title="新密码" :arrow="false">
					<input type="password" v-model="newPassword"  />
				</u-cell-item>
			</u-cell-group>
		</view>
		<u-button @click="save" >修改</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					newPassword:"",
					oldPassword:"",
					baseUrl:"",
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
			},
		methods: {
			save(){
				if(this.newPassword.length<6) {
					this.$u.toast("长度不得小于6")
					return
				}
				if(this.oldPassword.length<6) {
					this.$u.toast("长度不得小于6")
					return
				}
				this.$u.put("/prod-api/api/common/user/resetPwd",{
					"newPassword": this.newPassword,
					"oldPassword": this.oldPassword
				}).then(res=>{
					this.$u.toast("修改成功")
					this.newPassword=""
					this.oldPassword=""
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
