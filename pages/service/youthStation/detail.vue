<template>
	<view class="u-p-20">
		<u-swiper :list="swiperList" height="600"></u-swiper>
		<!-- <view class="u-m-t-20 u-m-b-20">地址：{{detail.address}}</view> -->
		<view class="page-box">
			<view class="order" >
				<view class="item" >
					<view class="content">
						<view class="title-lg">地址</view>
						<view class="">{{detail.address}}</view>
						<view class="title-lg">联系电话</view>
						<view class="">{{detail.phone}}</view>
						<view class="title-lg">办理入住时间段</view>
						<view class="">{{detail.workTime}}</view>
						<!-- <view class="title u-line-2">{{ detail.name }}</view> -->
						<view class="delivery-time u-flex u-row-between u-font-30">
							<view class="">剩余(男)：{{ detail.bedsCountBoy }}</view>
							<view class="">剩余(女)：{{ detail.bedsCountGirl }}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="title-lg">驿站简介</view>
		<view class="">{{detail.introduce}}</view>
		<view class="title-lg">房间配置</view>
		<view class="">{{detail.internalFacilities}}</view>
		<view class="title-lg">周边配套</view>
		<view class="">{{detail.surroundingFacilities}}</view>
		<view class="title-lg">特色服务</view>
		<view class="">{{detail.specialService}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					baseUrl:"电影封面图片、电影名称、评分、上映时间、想看人数等信息。底部展示主页按钮，点击“主页”，返回电影主页",
					detail:[],
					swiperList:[],
				}
			},
			onLoad(options){
				this.baseUrl = this.$u.http.config.baseUrl
				this.$u.get("/prod-api/api/youth-inn/youth-inn/"+options.id).then(res=>{
					this.detail = res.data
					console.log("detail",res)
					this.swiperList=this.detail.imageUrlList.map(item=>{
						return {image:this.baseUrl+item}
					})
				})
			},
		methods: {
			
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
				// font-size: 24rpx;
			}
		}
		.content>*{
			margin: 10rpx 0 10rpx 0;
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

