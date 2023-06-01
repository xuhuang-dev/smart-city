<style>
	.comment{
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
	<view class="u-p-20">
		<view class="u-p-20 u-border">
			<view class="u-text-center title-lg">{{detail.title}}</view>
			<rich-text :nodes="detail.content" class="u-content-color"></rich-text>
		</view>
		<u-gap height="80"></u-gap>
		<view class="u-flex comment" @click="$u.route({url:'/pages/home/comment',params:{id:detail.id}})">
			<u-icon name="chat" size="60"></u-icon>
			<input type="text" value="点击评论" class="u-text-center u-flex-1" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 218.7.112.123:10001
					// 124.93.196.45:10001
					baseUrl:"",
					detail:[],
				}
			},
			onLoad(options){
				this.baseUrl = this.$u.http.config.baseUrl
				this.$u.get("/prod-api/press/press/"+options.id).then(res=>{
					this.detail = res.data
				})
			},
		methods: {
			comment(){
				
			}
		}
	}
</script>

<style>

</style>
