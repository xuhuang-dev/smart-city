<template>
	<view class="u-p-20">
		<image src="../../../static/images/2.jpg" mode="" style="width: 100%;height: 300rpx;"></image>
		<!-- <text style="padding: 20rpx 0;">青年驿站介绍</text> -->
		<u-gap height="20"></u-gap>
		<u-grid col="3" :border="false">
			<u-grid-item v-for="(item,index) in serviceList" @click="$u.route({url:'/pages/service/youthStation/rencai',params:{id:item.id}})" class="u-flex-col">
				<image :src="baseUrl+item.imgUrl" mode="" style="height: 100rpx;width: 100rpx;border-radius: 50%;"></image>
				<text class="u-font-20 u-flex-1">{{item.name}}</text>
			</u-grid-item>
		</u-grid>
		<view class="page-box">
			<view class="order" v-for="(item, index) in newsList" :key="item.id" >
				<view class="item" @click="$u.route({url:'/pages/service/youthStation/detail',params:{id:item.id}})">
					<view class="left"><image :src="baseUrl+item.coverImgUrl" mode="aspectFill"></image></view>
					<view class="content">
						<view class="title u-line-2">{{ item.name }}</view>
						<view class="delivery-time u-flex u-row-between">
							<view class="">剩余(男)：{{ item.bedsCountBoy }}</view>
							<view class="">剩余(女)：{{ item.bedsCountGirl }}</view>
						</view>
						<view class="">地址：{{item.address}}</view>
					</view>
				</view>
				<u-collapse event-type="close"  >
					<u-collapse-item title="站点介绍" >
						{{item.introduce}}
					</u-collapse-item>
				</u-collapse>
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
			},
		methods: {
			getNews(){
				this.$u.get("/prod-api/api/youth-inn/youth-inn/list").then(res=>{
					// console.log(res)
					this.newsList = res.rows.map(item=>{
						return {...item,isDetail:false}
					})
					console.log("newsList",this.newsList)
				})
			},
			getService(){
				this.$u.get("/prod-api/api/youth-inn/talent-policy-area/list").then(res=>{
					console.log(res)
					this.serviceList =res.data
					console.log("serviceList",this.serviceList)
				})
			},
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

