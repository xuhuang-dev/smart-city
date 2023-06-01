<template>
	<view class="container">
		<view class="radiusWrap">
			<swiper class="swiper" :indicator-dots="true" :autoplay="true" :interval="2000" :duration="500">
				<swiper-item v-for="(item,index) in imglist" :key="index">
					<view class="swiper-item">
						<image style="width: 100%; height: 200px;" :src="base+item.imgUrl"></image>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="example-body">
			<view class="i-box" isShadow @click="clickCard">
				<rich-text :nodes="briefs"></rich-text>
			</view>
		</view>
		<button class="sub" type="primary" @click="clickSub">预约挂号</button>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				base: api.SERVER_BASE,
				imglist: [],
				briefs: ''
			}
		},
		onLoad(options) {
			this.getDetails(options.id);
		},
		methods: {
			getDetails(id) {
				this.$u.get(api.GET_HOSPITAL_DETAILS+id).then(res => {
					this.briefs = res.data.brief;
				});
				this.$u.get(api.GET_HOSPDTL_ROTATIONS+'?hospitalId='+id,{
					hospitalId: id
				}).then(res => {
					this.imglist = res.data;
				})
			},
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为：' + e.detail.value)
				this.index = e.detail.value
			},
			clickSub(){
				uni.navigateTo({
				    url: './personList'
				});
			}
		}
	}
</script>

<style>
	.radiusWrap{
		width: calc(100% - 30px);
		margin: 15px auto;
		border-radius: 10px;
		overflow: hidden;
	}
	.content-box-text{
		font-size: 14px;
		text-indent:50px;
	}
	.sub{
		margin-top: 10px;
		width: 90%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #6E78F7;
	}
	.uni-list{
		
	}
</style>
