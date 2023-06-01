<template>
	<view class="u-p-20">
		<image :src="baseUrl+detail.imgUrl" mode="" style="width: 100%;height: 300rpx;"></image>
		<view class="title-lg">简介</view>
		<view class="box">
			{{detail.introduce}}
		</view>
		<view class="title-lg">人才政策文件</view>
		<view class="page-box">
			<view class="order u-border-bottom" v-for="(item, index) in newsList" :key="item.id" >
				<view class="item " @click="$u.route({url:'/pages/service/youthStation/rencaiDetail',params:{id:item.id}})" >
					<view class="content">
						<view class="title u-line-2">{{ item.title }}</view>
						<view class="delivery-time u-flex u-row-between">
							<view class="">{{ item.createTime }}</view>
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
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					baseUrl:"电影封面图片、电影名称、评分、上映时间、想看人数等信息。底部展示主页按钮，点击“主页”，返回电影主页",
					detail:[],
					newsList:[],
				}
			},
			onLoad(options){
				this.baseUrl = this.$u.http.config.baseUrl
				console.log(this.detail)
				this.$u.get("/prod-api/api/youth-inn/talent-policy-area/"+options.id).then(res=>{
					this.detail = res.data
					this.getNews()
				})
			},
		methods: {
			getNews(){
				this.$u.get("/prod-api/api/youth-inn/talent-policy/list",{areaId:this.detail.id}).then(res=>{
					this.newsList = res.data
					console.log("newsList",this.newsList)
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

