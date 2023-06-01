<style>
	.set{
		position: absolute;
		top: 20rpx;
		right: 20rpx;
		color: white;
		font-size: 30rpx;
		padding: 20rpx;
	}
	.test{
		position: absolute;
		bottom: 120rpx;
		/* right: 20rpx; */
		color: white;
		font-size: 30rpx;
		padding: 20rpx;
	}
	
	.adv /deep/.uni-swiper-wrapper {
		height: 100vh;
	}
</style>
<template>
	<view class="adv">
		<u-swiper :list="swiperList" :autoplay="false"  mode="dot" :current="current" @change="change" style="height: 100vh;"></u-swiper>
		<view class="u-flex u-row-center" v-show="current==4">
			<view class="set" @click="isModal = true">设置IP</view>
			<view class="test" @click="test">立即体验</view>
		</view>
		<u-modal v-model="isModal" @confirm="save" :showCancelButton="true">
			<u-cell-group>
				<u-cell-item :arrow="false" title="IP">
					<input type="text" v-model="ip" />
				</u-cell-item>
			</u-cell-group>
		</u-modal>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 124.93.196.45:10001
				current:4,
				baseUrl:"",
				ip:"",
				swiperList:[
					{image:"../../static/images/0-1.jpg"},
					{image:"../../static/images/0-2.jpg"},
					{image:"../../static/images/0-3.jpg"},
					{image:"../../static/images/0-4.jpg"},
					{image:"../../static/images/0-5.jpg"},
				],
				winHeight:0,
				isModal:false,
			}
		},
		onLoad(){
			this.ip = uni.getStorageSync("ip")
			if(this.ip.length!=0){
				this.$u.route({url:"/pages/home/home",type:'tab'})
			}
			this.baseUrl = this.$u.http.config.baseUrl
			this.winHeight = uni.getSystemInfoSync().windowHeight
		},
		methods: {
			test(){
				if(this.ip.length==0){
					this.$u.toast("请输入IP和端口")
					this.isModal = true
					return;
				}
				uni.setStorageSync("ip",this.ip)
				this.$u.route({url:"/pages/home/home",type:'tab'})
			},
			save(){
				if(this.ip.length==0){
					this.$u.toast("请输入IP和端口")
					this.isModal = true
					return;
				}
			},
			change(e){
				this.current = e
			}
		}
	}
</script>

<style>

</style>
