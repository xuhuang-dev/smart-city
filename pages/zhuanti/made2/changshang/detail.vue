<template>
	<view class="u-p-20">
		<video :src="video" controls style="width: 100%;"></video>
		<!-- <u-swiper :list="swiperList"></u-swiper> -->
		<view class="title-lg">厂商介绍</view>
		<view class="">
			介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字介绍文字
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				video:"../../../../static/video/video01.mp4",
				swiperList:[
					{image:"../../../../static/images/dj1.jpg"},
					{image:"../../../../static/images/dj2.jpg"},
					{image:"../../../../static/images/dj3.jpg"},
				],
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
