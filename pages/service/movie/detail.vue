<template>
	<view>
		<view class="i-box">
			<view class="u-flex u-m-b-10" style="height: 200rpx;">
				<image :src="baseUrl+detail.cover" mode="" style="height: 100%;width: 200rpx;"></image>
				<view class="u-flex-col u-row-between u-flex-1 u-font-20 u-content-color u-m-10 u-p-20"
					style="height: 100%;">
					<view class="u-font-40 u-main-color u-line-2">{{detail.name}}</view>
					<view class="u-flex u-row-around">
						<view class="">上映时间</br>{{detail.playDate}}</view>
						<view class="">时长</br>{{detail.duration}}m</view>
						<view class="">想看人数</br>{{detail.likeNum}}</view>
					</view>
				</view>
			</view>
			<view class="u-font-32 u-m-30 u-flex u-row-between">
				评分<u-rate v-model="detail.score" disabled></u-rate>
			</view>
		</view>
		<view class="title-lg u-p-20">简介</view>
		<view class="i-box">
			<rich-text :nodes="detail.introduction"></rich-text>
		</view>
		<view class="title-lg u-p-20">影评</view>
		<view class="i-box u-flex-col u-row-center">
			<view class="comment" v-for="(item, index) in commentList" :key="item.id">
				<view class="left">
					<u-avatar></u-avatar>
				</view>
				<view class="right">
					<view class="top">
						<view class="name">{{ item.nickName }}</view>
						<view class="like" :class="{ highlight: item.isLike }">
							<view class="num">{{ item.likeNum }}</view>
							<u-icon v-if="!item.isLike" name="thumb-up" :size="30" color="#9a9a9a"
								@click="getLike(index)">
							</u-icon>
							<u-icon v-if="item.isLike" name="thumb-up-fill" :size="30" @click="getLike(index)"></u-icon>
						</view>
					</view>
					<rich-text :nodes="item.content" class="u-line-5"></rich-text>
				</view>
			</view>
			<u-button type="primary" shape="circle" size="medium" @click="currentNum++">查看更多影评
			</u-button>
		</view>
		<!-- <u-button type="primary" shape="circle" size="medium" @click="$u.route({type:'back',delta:1})">购买电影票</u-button> -->
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				// 218.7.112.123:10001
				// 124.93.196.45:10001
				baseUrl: "电影封面图片、电影名称、评分、上映时间、想看人数等信息。底部展示主页按钮，点击“主页”，返回电影主页",
				detail: [],
				commentList: [],
				currentNum: 1,
			}
		},
		watch:{
			currentNum(old){
				this.getComment(this.detail.id)
			}
		},
		onLoad(options) {
			this.baseUrl = this.$u.http.config.baseUrl
			console.log(this.detail)
			this.$u.get("/prod-api/api/movie/film/detail/" + options.id).then(res => {
				this.detail = res.data
				console.log(res)
			}).then(() => {
				this.getComment(this.detail.id)
			})
		},
		methods: {
			// 获取少量评论
			getComment(id) {
				this.$u.get(api.movieCommentList, {
					pageNum: this.currentNum,
					pageSize: 5,
					movieId: id
				}).then(res => {
					res.rows.forEach(item => {
						this.commentList.push({
							...item,
							isLike: false
						})
					})
				})
			},
			// 点赞
			getLike(index) {
				this.commentList[index].isLike = !this.commentList[index].isLike;
				if (this.commentList[index].isLike == true) {
					this.$u.post(api.movieCommentLike + this.commentList[index].id).then(res => {
						console.log(res)
						this.commentList[index].likeNum++;
					})
				} else {
					this.commentList[index].likeNum--;
				}
			},
		}
	}
</script>

<style lang="scss" scoped>
	.comment {
		display: flex;
		padding: 30rpx;

		.left {
			image {
				width: 64rpx;
				height: 64rpx;
				border-radius: 50%;
				background-color: #f2f2f2;
			}
		}

		.right {
			flex: 1;
			padding-left: 20rpx;
			font-size: 30rpx;

			.top {
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-bottom: 10rpx;

				.name {
					color: #5677fc;
				}

				.like {
					display: flex;
					align-items: center;
					color: #9a9a9a;
					font-size: 26rpx;

					.num {
						margin-right: 4rpx;
						color: #9a9a9a;
					}
				}

				.highlight {
					color: #5677fc;

					.num {
						color: #5677fc;
					}
				}
			}

			.content {
				margin-bottom: 10rpx;
			}

			.reply-box {
				background-color: rgb(242, 242, 242);
				border-radius: 12rpx;

				.item {
					padding: 20rpx;
					border-bottom: solid 2rpx $u-border-color;

					.username {
						font-size: 24rpx;
						color: #999999;
					}
				}

				.all-reply {
					padding: 20rpx;
					display: flex;
					color: #5677fc;
					align-items: center;

					.more {
						margin-left: 6rpx;
					}
				}
			}

			.bottom {
				margin-top: 20rpx;
				display: flex;
				font-size: 24rpx;
				color: #9a9a9a;

				.reply {
					color: #5677fc;
					margin-left: 10rpx;
				}
			}
		}
	}
</style>
