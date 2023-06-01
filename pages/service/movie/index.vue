<template>
	<view class="u-p-20">
		<u-swiper :list="swiperList" @click="$u.route({url:'/pages/service/movie/detail',params:{id:swiperList[$event].id}})"></u-swiper>
		<u-search @custom="search" @search="search" class="u-m-t-20" :clearabled="false" bg-color="white"></u-search>
		<view class="title-lg">电影信息展示区</view>
		<view class="page-box">
			<view class="order" v-for="(item, index) in newsList" :key="item.id" @click="$u.route({url:'/pages/service/movie/detail',params:{id:item.id}})">
				<view class="item" >
					<view class="left"><image :src="baseUrl+item.cover" mode="aspectFill"></image></view>
					<view class="content">
						<view class="title u-line-2">{{ item.name }}</view>
						<!-- <view class="type">{{ item.type }}</view> -->
						<view class="delivery-time u-flex u-row-between">
							<view class="">上映时间：{{item.playDate}}</view>
							<view class="">时长：{{item.duration}}m</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<u-button @click="lookMore" v-show="more" type="primary" shape="circle" size="medium" >查看更多</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				more:true,
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
					totalList:[],
				}
			},
			onLoad(){
				this.baseUrl = this.$u.http.config.baseUrl
				this.getSwiper()
				this.getNews()
			},
		methods: {
			lookMore(){
				
				this.newsList = this.totalList
				this.more = false
			},
			getNews(index){
				this.$u.get("/prod-api/api/movie/film/list").then(res=>{
					this.totalList = res.rows
					this.newsList = res.rows.slice(0,5)
					this.more = true
				})
			},
			search(e){
				if(e.length==0){
					this.getNews()
					return
				}
				this.$u.get("/prod-api/api/movie/film/list",{name:e}).then(res=>{
					if(res.total==0)					this.$u.toast("未找到")
					else{
						this.newsList = res.rows
						// this.$u.route({url:"/pages/service/movie/result",params:{name:e}})
					}
				})
			},
			getSwiper(){
				this.$u.get("/prod-api/api/movie/rotation/list").then(res=>{
					this.swiperList =res.rows.map(item=>{
						return {image:this.baseUrl+item.advImg,id:item.id}
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

