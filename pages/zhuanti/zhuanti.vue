<template>
	<view class="u-p-20">
		<u-swiper :list="swiperList"></u-swiper>
		<u-grid col="5" :border="false">
			<u-grid-item v-for="(item,index) in serviceList" @click="$u.route({url:'/pages/zhuanti/made2/'+item.link,type:index==9?'tab':'to'})" class="u-flex-col">
				<image :src="item.image" mode="" style="height: 100rpx;width: 100rpx;"></image>
				<text class="u-font-20 u-flex-1">{{item.serviceName}}</text>
			</u-grid-item>
		</u-grid>
		<view class="title-lg">推荐厂商</view>
		<view class="page-box">
			<view class="order" v-for="(res, index) in [1,2,3,4]" @click="$u.route({url:'/pages/zhuanti/made2/changshang/detail'})">
				<!-- <view class="top">
					<view class="left">
						<u-icon name="home" :size="30" color="rgb(94,94,94)"></u-icon>
						<view class="store">厂商名称</view>
						<u-icon name="arrow-right" color="rgb(203,203,203)" :size="26"></u-icon>
					</view>
					<view class="right">{{ res.deal }}</view>
				</view> -->
				<view class="item" v-for="(item, index2) in [1]" :key="index">
					<view class="left"><image :src="leftImage" mode=""></image></view>
					<view class="content">
						<view class="title">厂商名称</view>
						<view class="type">{{cnt[index]}}</view>
						<!-- <view class="type">{{ item.type }}</view>
						<view class="delivery-time">发货时间 {{ item.deliveryTime }}</view> -->
					</view>
					<!-- <view class="right">
						<view class="price">
							￥{{ priceInt(item.price) }}
							<text class="decimal">.{{ priceDecimal(item.price) }}</text>
						</view>
						<view class="number">x{{ item.number }}</view>
					</view> -->
				</view>
				<!-- <view class="total">
					共{{ totalNum(res.goodsList) }}件商品 合计:
					<text class="total-price">
						￥{{ priceInt(totalPrice(res.goodsList)) }}.
						<text class="decimal">{{ priceDecimal(totalPrice(res.goodsList)) }}</text>
					</text>
				</view>
				<view class="bottom">
					<view class="more"><u-icon name="more-dot-fill" color="rgb(203,203,203)"></u-icon></view>
					<view class="logistics btn">查看物流</view>
					<view class="exchange btn">卖了换钱</view>
					<view class="evaluate btn">评价</view>
				</view>
			 -->
			</view>
			<!-- <u-loadmore :status="loadStatus[0]" bgColor="#f2f2f2"></u-loadmore> -->
		</view>
		<view class="title-lg">推荐产品</view>
		<view class="page-box">
			<view class="order" v-for="(res, index) in [1,2,3]" @click="$u.route({url:'/pages/zhuanti/made2/product/detail'})">
				<view class="top">
					<!-- <view class="left">
						<u-icon name="home" :size="30" color="rgb(94,94,94)"></u-icon>
						<view class="store">产品名称</view>
						<u-icon name="arrow-right" color="rgb(203,203,203)" :size="26"></u-icon>
					</view> -->
					<!-- <view class="right">{{ res.deal }}</view> -->
				</view>
				<view class="item" v-for="(item, index) in [1]" :key="index">
					<view class="left"><image :src="leftImage" mode=""></image></view>
					<view class="content">
						<view class="title">产品名称</view>
						<view class="type">产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍产品介绍</view>
						<!-- <view class="type">{{ item.type }}</view>
						<view class="delivery-time">发货时间 {{ item.deliveryTime }}</view> -->
					</view>
					<!-- <view class="right">
						<view class="price">
							￥{{ priceInt(item.price) }}
							<text class="decimal">.{{ priceDecimal(item.price) }}</text>
						</view>
						<view class="number">x{{ item.number }}</view>
					</view> -->
				</view>
				<!-- <view class="total">
					共{{ totalNum(res.goodsList) }}件商品 合计:
					<text class="total-price">
						￥{{ priceInt(totalPrice(res.goodsList)) }}.
						<text class="decimal">{{ priceDecimal(totalPrice(res.goodsList)) }}</text>
					</text>
				</view>
				<view class="bottom">
					<view class="more"><u-icon name="more-dot-fill" color="rgb(203,203,203)"></u-icon></view>
					<view class="logistics btn">查看物流</view>
					<view class="exchange btn">卖了换钱</view>
					<view class="evaluate btn">评价</view>
				</view>
			 -->
			</view>
			<!-- <u-loadmore :status="loadStatus[0]" bgColor="#f2f2f2"></u-loadmore> -->
		</view>
			
	
	
	</view>
</template>

<script>
	export default {
		data() {
			return {
				leftImage:"../../static/images/image2.jpg",
				swiperList:[
					{image:"../../static/images/dj1.jpg"},
					{image:"../../static/images/dj2.jpg"},
					{image:"../../static/images/dj3.jpg"},
				],
				serviceList:[
					{image:"http://218.7.112.123:10001/prod-api/profile/upload/image/2021/05/10/814fc6c4-de48-48a1-95f8-de3e749e348d.png",link:"changshang/changshang",serviceName:"厂商列表"},
					{image:"http://218.7.112.123:10001/prod-api/profile/upload/image/2021/05/10/814fc6c4-de48-48a1-95f8-de3e749e348d.png",link:"product/product",serviceName:"产品列表"},
					{image:"http://218.7.112.123:10001/prod-api/profile/upload/image/2021/05/10/814fc6c4-de48-48a1-95f8-de3e749e348d.png",link:"active/active",serviceName:"展会活动"},
					{image:"http://218.7.112.123:10001/prod-api/profile/upload/image/2021/05/10/814fc6c4-de48-48a1-95f8-de3e749e348d.png",link:"zhaoping/zhaoping",serviceName:"厂商招聘"},
					{image:"http://218.7.112.123:10001/prod-api/profile/upload/image/2021/05/10/814fc6c4-de48-48a1-95f8-de3e749e348d.png",link:"join/join",serviceName:"厂商入驻"},
				],
				
				cnt: ['厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍厂商介绍'],
				changshangList:[
					{}
				]
			}
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
				border-radius: 10rpx;
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

