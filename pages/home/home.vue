<template>
	<view style="background-color: #f3f4f6;">
		<u-navbar :is-back="false" :background="{background:'#2979ff'}" :border-bottom="false" height="48">
			<view class="slot-wrap">
				<text style="color: white;font-weight: 600;" class="u-m-r-20 u-font-32">首页</text>
				<u-search input-align="center" :animation="true" bg-color="white" shape="square" @custom="search" @search="search" :clearabled="false" v-model="searchText"></u-search>
			</view>
		</u-navbar>
		
		<view style="padding:15px">
			<u-grid col="4" :border="false">
				<u-grid-item v-for="(item,index) in serviceList"
					@click="$u.route({url:'/pages/service/'+item.link,type:'to'})" class="u-flex-col"
					:customStyle="{padding:0,paddingBottom:index<4?'10px':0,backgroundColor:'#f3f4f6'}">
					<image :src="item.imgUrl" style="height: 80rpx;width: 80rpx;margin-bottom: 10rpx;"></image>
					<text class="">{{item.serviceName}}</text>
				</u-grid-item>
			</u-grid>
		</view>
		<i-box title="热门新闻">
			<u-swiper :list="swiperList"
				@click="$u.route({url:'/pages/home/detail',params:{id:swiperList[$event].id}})"></u-swiper>
		</i-box>
		<i-box title="热门专题" style="margin-top: 20rpx;margin-bottom: 20rpx;">
			<u-grid col="2" :border="false">
				<u-grid-item v-for="(item,index) in hotList"
					@click="$u.route({url:'/pages/home/detail',params:{id:item.id}})"
					:customStyle="{padding:0,paddingBottom:index<4?'10px':0}">
					<view class="u-flex-col box" :style="{marginRight:index/2==0?'10px':0}">
						<image :src="baseUrl+item.cover" mode="" style="height: 200rpx;width: 100%;"></image>
						<text class="u-font-20 u-flex-1 u-line-2">{{item.title}}</text>
					</view>
				</u-grid-item>
			</u-grid>
		</i-box>
		<u-sticky offset-top="8">
		<u-tabs :list="categoryList" :current="current" @change="categoryChange" class="u-m-b-20"></u-tabs>
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

	</view>
</template>

<script>
	export default {
		data() {
			return {
				searchText:"疫苗",
				offsetTop: 0,
				current: 0,
				// 218.7.112.123:10001
				// 124.93.196.45:10001
				content: "",
				title: "",
				baseUrl: "",
				swiperList: [],
				serviceList: [],
				hotList: [],
				categoryList: [],
				newsList: [],
				// newsList:[],
			}
		},
		onLoad() {
			this.baseUrl = this.$u.http.config.baseUrl
			this.getSwiper()
			this.getService()
			this.gethot()
			this.getCategory()
		},
		methods: {
			// 适配吸顶tab高度
			// setOffestTop() {
			// 	let systemInfo = uni.getSystemInfoSync()
			// 	let topPx = systemInfo.statusBarHeight + 44 // 顶部状态栏+沉浸式自定义顶部导航
			// 	this.offsetTop = topPx / (uni.upx2px(topPx) / topPx) // px转rpx
			// 	console.log(this.offsetTop)
			// },
			getNews(index) {//获取新闻列表
				this.$u.get("/prod-api/press/press/list", {
					type: this.categoryList[index].id
				}).then(res => {
					this.newsList = res.rows
					// console.log("newsList", this.newsList)
				})
			},
			categoryChange(index) {//切换新闻分类
				this.current = index
				this.getNews(index)
			},
			getCategory() {//获取新闻分类
				this.$u.get("/prod-api/press/category/list").then(res => {
					this.categoryList = res.data
					this.getNews(0)
					// console.log("categoryList",this.categoryList)
				})
			},
			gethot() {
				this.$u.get("/prod-api/press/press/list", {
					hot: "Y"
				}).then(res => {
					this.hotList = res.rows
				})
			},
			search(e) {
				if (e.length == 0) {
					this.$u.toast("请输入内容")
					return
				}
				this.$u.get("/prod-api/press/press/list", {
					title: e
				}).then(res => {
					if (res.total == 0) this.$u.toast("未找到")
					else {
						this.$u.route({
							url: "/pages/home/result",
							params: {
								title: e
							}
						})
					}
				})
			},
			getService() {
				this.$u.get("/prod-api/api/service/list").then(res => {
					let ids = [3, 5, 17, 18, 20, 21, 22, 26]
					// for(let i)
					res.rows.forEach(item => {
						if (ids.indexOf(item.id) > -1) this.serviceList.push({
							imgUrl: this.baseUrl + item.imgUrl,
							serviceName: item.serviceName,
							link: item.link
						})
					})
					// console.log(res.rows)
					// this.serviceList =res.rows.splice(0,7).map(item=>{
					// 	return {imgUrl:this.baseUrl+item.imgUrl,serviceName:item.serviceName,link:item.link}
					// })
					// this.serviceList.push({imgUrl:"/static/uview/example/more2.png",serviceName:"更多服务",link:"service"})
					console.log("serviceList", this.serviceList)
				})
			},
			getSwiper() {
				this.$u.get("/prod-api/api/rotation/list", {
					type: 2
				}).then(res => {
					this.swiperList = res.rows.map(item => {
						return {
							image: this.baseUrl + item.advImg,
							id: item.id
						}
					})
				})
			}
		}
	}
</script>

<style>
	.i-box /deep/ .u-grid-item-box {
		padding: 0;
	}
</style>
<style lang="scss" scoped>
	.slot-wrap {
		height: 48px;
		display: flex;
		align-items: center;
		/* 如果您想让slot内容占满整个导航栏的宽度 */
		flex: 1;
		/* 如果您想让slot内容与导航栏左右有空隙 */
		padding: 0 20rpx;
		// background-color: #2979ff;
	}

	.order {
		width: 710rpx;
		background-color: #ffffff;
		// margin: 20rpx auto;
		margin: auto;
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
