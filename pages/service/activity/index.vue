<template>
	<view class="container u-p-20">
		<u-swiper :list="swiperList" @click="$u.route({url:'/pages/service/activity/activityDetail',params:{id:swiperList[$event].id}})"></u-swiper>
		<u-sticky>
			<u-tabs :list="categoryList" :current="current" @change="categoryChange" class="u-m-b-20"></u-tabs>
		</u-sticky>
		<view class="page-box">
			<view class="order" v-for="(item, index) in resList" :key="item.id"
				@click="$u.route({url:'/pages/service/activity/activityDetail',params:{id:item.id}})"
				:style="{marginBottom:index<(resList.length-1)?'20rpx':0}">
				<view class="item">
					<view class="left">
						<image :src="baseUrl+item.imgUrl" mode="aspectFill"></image>
					</view>
					<view class="content u-flex-1 u-flex-col u-row-between">
						<view class="title u-line-2">{{ item.name }}</view>
						<view class="delivery-time u-flex u-row-between">
							<view class="">参与人数：{{ item.signupNum }}</view>
							<view class="">点赞数：{{item.likeNum}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import api from "../../../common/apiUtil.js"
	export default {
		data() {
			return {
				baseUrl: "",
				swiperList:[],
				categoryList: [],
				resList: [],
				current: 0,
			}
		},
		methods: {
			getActivityList(id) {
				this.$u.get(api.activityList, {
					categoryId: id
				}).then(res => {
					this.resList = res.rows
					// console.log("resList", this.resList)
				})
			},
			categoryChange(index) {
				this.current = index
				this.getActivityList(this.categoryList[index].id)
			},
			getSwiperList(){
				this.$u.get(api.activityList,{pageSize:5,pageNum:2}).then(res=>{
					this.swiperList = res.rows.map(item=>{
						return {...item,image:this.baseUrl+item.imgUrl}
					})
					console.log(this.swiperList)
				})
			},
			getCategoryList() {
				this.$u.get(api.activity_category_list).then(res => {
					// console.log(res)
					this.categoryList = res.data
					// this.getactivity_list(this.categoryIndex)
				})
			},
			goToDetail(item) {
				uni.navigateTo({
					url: 'acticityDetail?id=' + item.id
				})
			}
		},
		mounted() {},
		async onLoad() {
			this.baseUrl = this.$u.http.config.baseUrl
			this.getSwiperList()
			await this.getCategoryList()
			this.getActivityList(1)

		}
	}
</script>
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
