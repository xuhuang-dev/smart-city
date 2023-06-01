<template>
	<view class="u-p-20">
		<view class="box u-flex u-m-b-10" v-for="(item,index) in newsList" @click="$u.route({url:'/pages/home/detail',params:{id:item.id}})" style="height: 200rpx;">
			<image :src="baseUrl+item.cover" mode="" style="height: 100%;width: 200rpx;"></image>
			<view class="u-flex-col u-row-between u-flex-1 u-font-20  u-content-color ">
				<view class="u-font-30 u-main-color u-line-2">{{item.title}}</view>
				<rich-text :nodes="item.content" class="u-line-2"></rich-text>
				<view class="u-flex u-row-around">
					<view class="">评论数：{{item.commentNum}}</view>
					<view class="">{{item.publishDate}}</view>
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
					baseUrl:"",
					newsList:[],
					// newsList:[],
				}
			},
			onLoad(options){
				this.baseUrl = this.$u.http.config.baseUrl
				this.$u.get("/prod-api/api/movie/film/list",{name:options.name}).then(res=>{
					this.newsList = res.rows
				})
			},
		methods: {
			
		}
	}
</script>

<style>

</style>
