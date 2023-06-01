<template>
	<view>
		<view class="u-flex user-box u-p-l-30 u-p-r-20 u-p-b-30">
			<view class="u-m-r-10" style="margin-top: 40rpx;">
				<u-avatar :src="baseUrl+info.avatar" size="140"></u-avatar>
			</view>
			<view class="u-flex-1">
				<view class="u-font-14 u-tips-color">微信号:{{info.userName}}</view>
			</view>
		</view>
		<view class="">
			<u-cell-group>
				<u-cell-item @click="$u.route('/pages/user/info')" title="个人信息"></u-cell-item>
				<u-cell-item @click="$u.route('/pages/user/password')" title="修改密码"></u-cell-item>
				<u-cell-item @click="$u.route('/pages/user/feedback')" title="意见反馈"></u-cell-item>
				<u-cell-item @click="$u.route('/pages/user/order2')" title="订单"></u-cell-item>
			</u-cell-group>
		</view>
		
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item @click="quit" title="退出" :arrow="false"></u-cell-item>
			</u-cell-group>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
					// 124.93.196.45:10001
					username:"",
					password:"",
					baseUrl:"",
					info:{},
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
			},
			onShow(){
				this.$u.get("/prod-api/api/common/user/getInfo").then(res=>{
					this.info = res.user
				})
			},
		methods: {
			quit(){
				uni.removeStorageSync("token")
				this.$u.route("/pages/login/login")
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
