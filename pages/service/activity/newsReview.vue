<template>
	<view class="reviewWrap">
		<view class="reviewHead">
			<input v-model="currMsg" placeholder="说说你的看法" />
			<button @click="submitRw" type="primary">评论</button>
		</view>
		<view class="reviewAll">
			<h3>最新评论</h3>
			<view v-if="reviewMsg.length>0" >
				<view class="comment" v-for="(item,index) in reviewMsg" :key="index">
					{{item.nickName}} : {{item.content}}<br>{{item.commentTime}}
				</view>
			</view>
			<view class="noRwData" v-else>暂无评论！</view>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	// import dlReview from '../../components/dl-review/review.vue';
	export default {
		data() {
			return {
				currMsg: '',
				reviewMsg: [],
				commentId: 0
			}
		},
		// components:{
		// 	dlReview
		// },
		onLoad(options) {
			this.getComments(options.id);
			this.commentId = options.id;
		},
		methods: {
			getComments(id) {
				this.reviewMsg = [];
				this.$u.get("/prod-api/api/activity/comment/list",{
					activityId: id
				}).then(res => {
					// console.log(res.rows)
					this.reviewMsg = res.rows
					// console.log(this.reviewMsg)
				})
			},
			submitRw() {
				if(this.currMsg.length == 0) {
					uni.showToast({
						title: '请先输入评论内容！',
						icon: 'none'
					})
				} else {
					let that = this;
					uni.getStorage({
					    key: 'info',
					    success: function (res) {
							that.$u.post(api.ADD_activity_COMMENTS,{
								activityId: that.commentId,
								content: that.currMsg
							}).then(res => {
								that.getComments(that.commentId);
							});
							that.currMsg = '';
					    },
						fail: function() {
							uni.showToast({
								title: '提交失败，请重试！',
								icon: 'none'
							});
						}
					});
					
					
				}
			},
			getTime() {
				var today = new Date();
				var day = today.getDate();
				var month = today.getMonth() + 1;
				var year = today.getFullYear();
				if (month<10) month="0"+month;
				if (day<10) day = "0"+day;
				var date = year + " / " + month + " / " + day;
				var h = today.getHours();       //获取当前小时数(0-23)
				if (h<10)h="0"+h;
				var m = today.getMinutes();
				var sec = today.getSeconds();
				if (m<10)m="0"+m;
				if (sec<10) sec="0"+sec;
				var timeCont = year+'-'+month+'-'+day+' '+h+':'+m+':'+sec;
				return timeCont;
			}
		}
	}
</script>

<style>
	.comment{
		border: 1px solid #3C3E49;
		display: flex;
		flex-direction: column;
		margin: 20rpx;
		padding: 10rpx;
		border-radius: 10rpx;
		
	}
	.reviewHead{
		width: calc(100% - 30px);
		margin: 10px auto;
		display: flex;
	}
	.reviewHead input{
		flex: auto;
		height: 40px;
		border: 1rpx #D3D3D3 solid;
		background-color: #fff;
		border-radius: 5px 0 0 5px;
		text-indent: 1em;
	}
	.reviewHead button{
		height: 41px;
		font-size: 15px;
		line-height: 40px;
		border-radius: 0 5px 5px 0;
	}
	.reviewAll{
		width: 100%;
		background-color: #fff;
	}
	.reviewAll h3{
		font-size: 18px;
		line-height: 48px;
		text-indent: 1em;
		font-weight: 500;
	}
	.noRwData{
		padding: 20px 0;
		text-align: center;
	}
</style>
