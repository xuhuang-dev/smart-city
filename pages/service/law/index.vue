<template>
	<view class="u-p-20">
		<u-search @custom="search" @search="search" class="u-m-b-20" :clearabled="false"></u-search>
		<u-swiper :list="swiperList" @click="$u.route({url:'/pages/home/detail',params:{id:swiperList[$event].id}})"></u-swiper>
		<swiper :autoplay="false" style="height: 400rpx;">
			<swiper-item v-for="(item,index) in serviceList" >
				<u-grid col="4" :border="false" >
					<u-grid-item v-for="(item1,index2) in item" @click="$u.route({url:'/pages/service/law/legal-expertise',params:{id:item1.id}})" class="u-flex-col" >
						<image :src="baseUrl+item1.imgUrl" mode="" style="height: 100rpx;width: 100rpx;border-radius: 50%;"></image>
						<text class="u-font-20 u-flex-1">{{item1.name}}</text>
					</u-grid-item>
				</u-grid>
			</swiper-item>
		</swiper>
		<view class="title-lg">本月上榜优选律师</view>
		<!-- `头像、名称、从业年限、咨询人数、法律专长、好评率和咨询按钮` -->
		<view class="page-box">
			<view class="order" v-for="(item, index) in newsList" :key="item.id" >
				<view class="item" @click="$u.route({url:'/pages/service/youthStation/detail',params:{id:item.id}})">
					<view class="left"><image :src="baseUrl+item.avatarUrl" mode="aspectFill"></image></view>
					<view class="content u-flex-1">
						<view class="title u-line-2">{{ item.name }}</view>
						<view class="u-font-20 u-flex u-row-between">
							<view class="">从业年限：{{ 22-item.workStartAt.substr(2,2) }}</view>
						</view>
						<view class="u-font-20 u-flex u-row-between">
							<view class="">法律专长：{{ item.legalExpertiseName }}</view>
						</view>
						<view class="delivery-time u-flex u-row-between">
							<view class="">咨询人数：{{ item.serviceTimes }}</view>
						</view>
						<view class="delivery-time u-flex u-row-between">
							<view class="">好评率：{{ item.favorableRate }}</view>
						</view>
					</view>
					<view class="right u-flex u-row-center"><u-button size="mini" type="error">咨询</u-button></view>
				</view>
				<!-- <u-collapse event-type="close"  >
					<u-collapse-item title="站点介绍" >
						{{item.introduce}}
					</u-collapse-item>
				</u-collapse> -->
			</view>
		</view>
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
				current:0,
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					content:"",
					title:"",
					baseUrl:"",
					swiperList:[],
					serviceList:[],
					hotList:[],
					categoryList:[],
					newsList:[],
					// newsList:[],
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
				this.getNews()
				this.getService()
				this.getSwiper()
			},
		methods: {
			search(e){
				if(e.length==0){
					this.$u.toast("请输入内容")
					return
				}
				this.$u.get("/prod-api/press/press/list",{title:e}).then(res=>{
					if(res.total==0)					this.$u.toast("未找到")
					else{
						this.$u.route({url:"/pages/home/result",params:{title:e}})
					}
				})
			},
			getNews(){
				this.$u.get("/prod-api/api/lawyer-consultation/lawyer/list-top10").then(res=>{
					// console.log(res)
					this.newsList = res.data
					console.log("newsList",this.newsList)
				})
			},
			getService(){
				this.$u.get("/prod-api/api/lawyer-consultation/legal-expertise/list").then(res=>{
					console.log(res)
					this.serviceList.push(res.rows.slice(0,8))
					this.serviceList.push(res.rows.slice(8,16))
					console.log("serviceList",this.serviceList)
				})
			},
			getSwiper(){
				this.$u.get("/prod-api/api/lawyer-consultation/ad-banner/list").then(res=>{
					console.log("swiper",res)
					this.swiperList =res.data.map(item=>{
						return {image:this.baseUrl+item.imgUrl,id:item.id}
					})
				})
			}
		}
	}
</script>


<style lang="scss" scoped>
.order {
	width: 710rpx;
	background-color: #ffffff;
	margin: 20rpx auto;
	border-radius: 20rpx;
	box-sizing: border-box;
	padding: 20rpx;
	font-size: 28rpx;
	.top {
		display: flex;
		justify-content: space-between;
		.left {
			display: flex;
			align-items: center;
			.store {
				margin: 0 10rpx;
				font-size: 32rpx;
				font-weight: bold;
			}
		}
		.right {
			color: $u-type-warning-dark;
		}
	}
	.item {
		display: flex;
		margin: 20rpx 0 0;
		.left {
			margin-right: 20rpx;
			image {
				width: 200rpx;
				height: 200rpx;
				// border-radius: 10rpx;
			}
		}
		.content {
			.title {
				font-size: 28rpx;
				line-height: 50rpx;
			}
			.type {
				margin: 10rpx 0;
				font-size: 24rpx;
				color: $u-tips-color;
			}
			.delivery-time {
				color: #e5d001;
				font-size: 24rpx;
			}
		}
		.right {
			margin-left: 10rpx;
			padding-top: 20rpx;
			text-align: right;
			.decimal {
				font-size: 24rpx;
				margin-top: 4rpx;
			}
			.number {
				color: $u-tips-color;
				font-size: 24rpx;
			}
		}
	}
	.total {
		margin-top: 20rpx;
		text-align: right;
		font-size: 24rpx;
		.total-price {
			font-size: 32rpx;
		}
	}
	.bottom {
		display: flex;
		margin-top: 40rpx;
		padding: 0 10rpx;
		justify-content: space-between;
		align-items: center;
		.btn {
			line-height: 52rpx;
			width: 160rpx;
			border-radius: 26rpx;
			border: 2rpx solid $u-border-color;
			font-size: 26rpx;
			text-align: center;
			color: $u-type-info-dark;
		}
		.evaluate {
			color: $u-type-warning-dark;
			border-color: $u-type-warning-dark;
		}
	}
}
.centre {
	text-align: center;
	margin: 200rpx auto;
	font-size: 32rpx;
	image {
		width: 164rpx;
		height: 164rpx;
		border-radius: 50%;
		margin-bottom: 20rpx;
	}
	.tips {
		font-size: 24rpx;
		color: #999999;
		margin-top: 20rpx;
	}
	.btn {
		margin: 80rpx auto;
		width: 200rpx;
		border-radius: 32rpx;
		line-height: 64rpx;
		color: #ffffff;
		font-size: 26rpx;
		background: linear-gradient(270deg, rgba(249, 116, 90, 1) 0%, rgba(255, 158, 1, 1) 100%);
	}
}
.wrap {
	display: flex;
	flex-direction: column;
	height: calc(100vh - var(--window-top));
	width: 100%;
}
.swiper-box {
	flex: 1;
}
.swiper-item {
	height: 100%;
}
</style>

