<template>
	<view class="detailWrap">
		<h2>{{title}}</h2>
		<view @click="takeActivity" class="takeBtn" >{{takeTip}}</view>
		<view class="detailCont">
			<image :src="imgSrc" mode="widthFix"></image>
			<rich-text :nodes="content"></rich-text>
		</view>
		<view class="tuijian">
			<view class="tuijian-title">推荐活动</view>
			<view class="content-total">
				<view @click="goToDetail(item)" class="new-for-in" v-for="(item,index) in activity_list" :key="index">
					<view style="display: flex;">
						<view style="	display: flex;">
							<image class="new-item-cover" :src="baseUrl+item.imgUrl" mode=""></image>
						</view>
						<view class="right_title">
							<text class="new-item-title">{{item.name}}</text>
							<view class="title_mid"></view>
							<view class="title_bottom">
								<text style="margin-right: 10rpx;">参与人数：{{item.signupNum}}</text>
								<text>点赞数：{{item.likeNum}}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<navigator :url="'./newsReview?id='+cmId">
			<view class="reviewBox">
				<!-- <view class="writeIcon"> -->
					<u-icon name="edit-pen" size="50" color="#c0c4cc"></u-icon>
				<!-- </view> -->
				<view class="reviewInput">
					<input placeholder="说说你的看法" disabled="false" />
				</view>
				<view class="reviewNum" style="position: relative">
					<u-icon name="chat" size="60" color="#c0c4cc" >
					</u-icon>
						<u-badge :is-center="true" :count="commentNum" type="error" size="mini"></u-badge>
				</view>
			</view>
		</navigator>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				baseUrl: api.SERVER_BASE,
				title: '',
				content: '',
				cmId: 0,
				imgSrc: '',
				commentNum: 0,
				take:0,
				token:'',
				activity_list:[],
			}
		},
		onLoad(options) {
			this.getDetails(options.id);
			this.cmId = options.id;
			this.getCommentNum(options.id);
		},
		methods: {
			getDetails(id) {
				this.$u.get(api.activityDetail+'/'+id).then(res => {
					console.log(res)
					this.title = res.data.name;
					this.content = res.data.content;
					this.imgSrc = this.baseUrl+res.data.imgUrl;
				})
			},
			getCommentNum(id) {
				this.$u.get(api.activityCommentNum,{
					activityId: id
				}).then(res => {
					console.log(res)
					this.commentNum = res.commentNum;
				})
			},
			check(){
				this.$u.get(api.checksignup,{
					activityId:this.cmId
				}).then(res => {
					console.log(res)
					if(res.isSignup){
						this.take = res.isSignup
					}
				})
			},
			takeActivity(){
				this.$u.post(api.signup,{activityId:this.cmId}).then((res)=>{
					if(res.code==200){
						this.take = 1
						this.$u.toast(res.msg)
					}
				})
			},
			getActivityList(){
				uni.request({
					url: api.activityList,
					method: 'GET',
					data: {},
					success: res => {
						// console.log(res)
						this.activity_list = res.data.rows.splice(0,3)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goToDetail(item){
				uni.navigateTo({
					url:'acticityDetail?id='+item.id
				})
			}
		},
		computed:{
			takeTip(){
				if(this.take)
					return "已报名"
				else
					return "报名"
			}
		},
		mounted() {
			this.token = uni.getStorageSync('token')
			// console.log(this.token)
			this.getActivityList()
			this.check()
		}
	}
</script>

<style>
.content-total{
	/* padding: 20rpx; */
	width: 100%;
	background-color: white;
	/* height: 2000px; */
}
.new-for-in{
	width: 90%;
	margin: auto;
	margin-bottom: 20rpx;
	overflow: hidden;
	border-radius: 20rpx;
	background-color: #e7ecf4;
	/* border-bottom: 1px solid #e3e3e3; */
	/* margin-right: 20rpx; */
	/* background-color: #18BC37; */
	/* padding: 30rpx; */
	
	
}
.new-item-cover{
	display: flex;
	width: 180rpx;
	height: 180rpx;
}
.right_title{
	font-size: 30rpx;
	display: flex;
	flex-direction: column;
	height: 180rpx;
}
.new-item-title{
	margin: 10rpx 20rpx ;
	display: -webkit-box;
	-webkit-line-clamp:2;
	-webkit-box-orient:vertical;
	overflow: hidden;
}
.title_mid{
	flex: 1;
}
.title_bottom{
	margin-left: 15rpx;
	font-size: 24rpx;
	line-height: 40rpx;
	/* background-color: #1890FF; */
}
// 
.tuijian-title{
	
	margin: 30rpx auto;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}
.tuijian{
	width: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}
	.takeBtn{
		display: inline-block;
		border-radius: 10rpx;
		padding: 10rpx 20rpx;
		background-color: #1890FF;
	}
	.detailWrap{
		width: 750rpx;
		box-sizing: border-box;
		padding: 10px 15px;
		min-height: 100vh;
		background-color: #fff;
		padding-bottom: 50px;
	}
	.detailWrap>h2{
		font-size: 18px;
		line-height: 30px;
		color: #3B4144;
		text-align: center;
		padding: 10px 0;
	}
	.detailCont{
		width: 100%;
		box-sizing: border-box;
		padding: 20px 15px;
		background-image: linear-gradient(to bottom,#FEFEFE,#E3EFF8);
		box-shadow: 0px 0px 5px #D3D3D3;
	}
	.detailCont image{
		width: 100%;
		background: no-repeat;
	}
	.reviewBox{
		width: 100%;
		height: 50px;
		padding: 0 15px;
		box-sizing: border-box;
		background-color: #fff;
		position: fixed;
		left: 0; bottom: 0;
		z-index: 9;
		display: flex;
		justify-content: space-between;
		align-items: center;
		box-shadow: -3px 0 3px rgba(0,0,0,0.4);
	}
	.reviewInput{
		flex: auto;
		padding-left: 8px;
	}
	.reviewNum{
		position: relative;
	}
	.rwBadge{
		position: absolute;
		left: 12px; top: -8px;
	}
</style>
