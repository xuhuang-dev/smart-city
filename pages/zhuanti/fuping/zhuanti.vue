<template>
	<view class="u-p-20">
		<u-swiper class="u-m-b-20"></u-swiper>
		<!-- <view class="title-lg">推荐服务</view> -->
		<u-grid col="5" :border="false">
			<u-grid-item v-for="(item,index) in serviceList" @click="$u.route({url:'/pages/zhuanti/fuping/'+item.link,type:index==9?'tab':'to'})" class="u-flex-col">
				<image :src="baseUrl+item.imgUrl" mode="" style="height: 100rpx;width: 100rpx;"></image>
				<text class="u-font-20 u-flex-1">{{item.serviceName}}</text>
			</u-grid-item>
		</u-grid>
		<view class="title-lg">扶贫案例</view>
		<view class="box u-flex u-m-b-10" v-for="(item,index) in [1,2,3,4]" @click="$u.route({url:'/pages/home/detail',params:{id:item.id}})" style="height: 200rpx;background-color: #ccc;">
			<image :src="baseUrl+item.cover" mode="" style="height: 100%;width: 200rpx;"></image>
			<view class="u-flex-col u-row-between u-flex-1 u-font-20  u-content-color u-m-10" >
				<view class="u-font-30 u-main-color u-line-2">{{item.title}}</view>
				<rich-text :nodes="item.content" class="u-line-2"></rich-text>
				<view class="u-flex u-row-around">
					<view class="">评论数：{{item.commentNum}}</view>
					<view class="">点赞：{{item.publishDate}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				serviceList:[
					{serviceName:"扶贫案例",link:"case",imgUrl:""},
					{serviceName:"村情村貌",link:"mao",imgUrl:""},
					{serviceName:"案例发布",link:"push",imgUrl:""},
					{serviceName:"求助信息",link:"helpMsg",imgUrl:""},
					{serviceName:"入户走访",link:"act",imgUrl:""},
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
