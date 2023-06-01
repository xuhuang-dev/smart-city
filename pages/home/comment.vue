<style>
	.comment-self{
		position: fixed;
		bottom: 0;
		right: 0;
		left: 0;
		background-color: white;
		padding: 20rpx;
		z-index: 99;
	}
</style>
<template>
	<view>
		<view class="comment" v-for="(res, index) in commentList" :key="res.id">
			<view class="left">
				<u-avatar :src="res.url"></u-avatar>
			</view>
			<view class="right">
				<view class="top">
					<view class="name">{{ res.userName }}</view>
					<view class="like" :class="{ highlight: res.isLike }">
						<view class="num">{{ res.likeNum }}</view>
						<u-icon v-if="!res.isLike" name="thumb-up" :size="30" color="#9a9a9a" @click="getLike(index)"></u-icon>
						<u-icon v-if="res.isLike" name="thumb-up-fill" :size="30" @click="getLike(index)"></u-icon>
					</view>
				</view>
				<view class="content">{{ res.content }}</view>
				
			</view>
		</view>
		<u-gap height="80"></u-gap>
		<view class="u-flex comment-self" >
			<u-icon name="chat" size="60" ></u-icon>
			<input type="text" v-model="commentContent" placeholder="发条评论吧" class="u-flex-1" />
			<u-button @click="sent">发送</u-button>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			commentList: [],
			baseUrl:"",
			detail:[],
			selfList:[],
			commentContent:"",
			id:0,
			}
		},
		onLoad(options){
			this.baseUrl = this.$u.http.config.baseUrl
			this.id = options.id
			this.getComment();
	},
	// "Failed to convert value of type 'java.lang.String' to required type 'java.lang.Long'; 
	// nested exception is java.lang.NumberFormatException: For input string: "undefined""
	methods: {
		getComment(){
			this.$u.get("/prod-api/press/comments/list",{newsId:this.id}).then(res=>{
				this.commentList = res.rows.map(item=>{
					return {...item,isLike:false}
				})
				console.log("comment",res)
			})
		},
		// 跳转到全部回复
		toAllReply() {
			uni.navigateTo({
				url: '/pages/template/comment/reply'
			});
		},
		// 点赞
		getLike(index) {
			this.commentList[index].isLike = !this.commentList[index].isLike;
			if (this.commentList[index].isLike == true) {
				this.$u.put("/prod-api/press/pressComment/like/"+this.commentList[index].id).then(res=>{
					console.log(res)
					this.commentList[index].likeNum++;
				})
			} else {
				this.commentList[index].likeNum--;
			}
		},
		sent(){
				this.$u.post("/prod-api/press/pressComment",{
					"newsId": this.id,
					"content": this.commentContent
				}).then(res=>{
					console.log(res)
					// this.getComment()
					if(res.code == 200 ){
						this.$u.toast("评论成功")
						this.commentList.unshift({id:1,userName:'xuhaung',isLike:false,likeNum:0,content:this.commentContent})
						this.commentContent = ""
					}
				})
			
		},
	}
};
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

