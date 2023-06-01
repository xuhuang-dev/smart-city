<template>
	<view class="u-p-20">
		<u-search class="u-m-b-20" bg-color="white" shape="square" @custom="search" @search="search" :clearabled="false" :action-style="{color:'#2979FF',borderLeft:'4rpx solid #2979FF',fontWeight:'600'}"></u-search>
		<u-swiper class="u-m-b-20" :list="swiperList" @click="$u.route({url:'/pages/home/detail',params:{id:swiperList[$event].id}})"></u-swiper>
		<u-sticky>
			<u-tabs :list="categoryList" :current="current" @change="categoryChange"></u-tabs>
		</u-sticky>
		<view class="page-box">
			<view class="order" v-for="(item, index) in newsList" :key="item.id"
				@click="$u.route({url:'/pages/home/detail',params:{id:item.id}})"
				:style="{marginBottom:index<(newsList.length-1)?'20rpx':0}">
				<view class="item">
					<view class="left">
						<image :src="baseUrl+item.cover" mode="aspectFill"></image>
					</view>
					<view class="content">
						<view class="title u-line-2">{{ item.title }}</view>
						<rich-text :nodes="item.content" class="type u-line-2"></rich-text>
						<!-- <view class="type">{{ item.type }}</view> -->
						<view class="delivery-time u-flex u-row-between">
							<view class="">评论数：{{ item.commentNum }}</view>
							<view class="">{{ item.publishDate }}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- <view class="box u-flex u-m-b-10" v-for="(item,index) in newsList" @click="$u.route({url:'/pages/home/detail',params:{id:item.id}})" style="height: 200rpx;">
			<image :src="baseUrl+item.cover" mode="" style="height: 100%;width: 200rpx;"></image>
			<view class="u-flex-col u-row-between u-flex-1 u-font-20  u-content-color ">
				<view class="u-font-30 u-main-color u-line-2">{{item.title}}</view>
				<rich-text :nodes="item.content" class="u-line-2"></rich-text>
				<view class="u-flex u-row-around">
					<view class="">评论数：{{item.commentNum}}</view>
					<view class="">{{item.publishDate}}</view>
				</view>
			</view>
		</view> -->
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
				this.getSwiper()
				this.getCategory()
			},
		methods: {
			getNews(index){
				this.$u.get("/prod-api/press/press/list",{type:this.categoryList[index].id}).then(res=>{
					this.newsList = res.rows
					console.log("newsList",this.newsList)
				})
			},
			categoryChange(index){
				this.current = index
				this.getNews(index)
			},
			getCategory(){
				this.$u.get("/prod-api/press/category/list").then(res=>{
					this.categoryList = res.data
					this.getNews(0)
					// console.log("categoryList",this.categoryList)
				})
			},
			search(e){
				if(e.length==0){
					this.$u.toast("请输入内容")
					return
				}
				this.$u.get("/prod-api/press/press/list",{title:e}).then(res=>{
					if(res.total==0){
						this.$u.toast("未找到")
					}
					else{
						this.$u.route({url:"/pages/home/result",params:{title:e}})
					}
				})
			},
			getSwiper(){
				this.$u.get("/prod-api/api/rotation/list",{type:2}).then(res=>{
					this.swiperList =res.rows.map(item=>{
						return {image:this.baseUrl+item.advImg,id:item.id}
					})
				})
			}
		}
	}
</script>
<style >
	/deep/ .u-search{
		background-color: white;
	}
</style>
<style lang="scss" scoped>
.order {
	width: 710rpx;
	background-color: #ffffff;
	margin: 20rpx auto;
	/* margin:auto; */
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
