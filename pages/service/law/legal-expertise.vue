<template>
	<view>
		
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
					swiperList:[],
				}
			},
			onLoad(options){
				this.baseUrl = this.$u.http.config.baseUrl
				this.$u.get("/prod-api/api/youth-inn/youth-inn/"+options.id).then(res=>{
					this.detail = res.data
					console.log("detail",res)
					this.swiperList=this.detail.imageUrlList.map(item=>{
						return {image:this.baseUrl+item}
					})
					console.log(imgList)
				})
			},
		methods: {
			
		}
	}
</script>

<style>

</style>
