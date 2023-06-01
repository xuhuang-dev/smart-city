<template>
	<view>
		<view class="example-body">
			<u-card :extra="info.distance+'公里'" isShadow :bodyStyle="{padding:0}">
				<view slot="head">
				<text>{{info.parkName}}</text>
				</view>
				<view style="padding: 12px;" slot="body">
					<view style="line-height: 30px;">对外开放</view>
					<view class="postion">
						<image src="../../../static/icons/postion.png" class="postionIcon"></image>
						<text style="color: #666666;line-height: 30px;">{{info.address}}</text>
					</view>
					<view style="line-height: 40px;">车位信息</view>
					<view class="postionDesc">
						<view class="desc1">
							<text class="textcenter" style="color: #666666;">停车费\n</text>
							<text class="textcenter" style="color: #FFA237;"><text style="font-size: 24px;">{{info.rates}}元</text>/小时</text>
						</view>
						<view class="desc1">
							<text class="textcenter" style="color: #666666;">剩余车位\n</text>
							<text class="textcenter" style="color: #3D4FF4;"><text style="font-size: 24px;">{{info.vacancy}}个</text>/{{info.allPark}}</text>
						</view>
					</view>
				</view>
				<view class="descBottom" style="clear: both;">
					<view>收费参考</view>
					<view>每小时{{info.rates}}元，最高{{info.priceCaps}}一天</view>
				</view>
			</u-card>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				info:{}
			}
		},
		onLoad(options) {
			this.getParkDetails(options.id)
		},
		methods: {
			getParkDetails(id) {
				this.$u.get(api.GET_PARK_DETAILS+id).then(res => {
					this.info = res.data;
				})
			}
		}
	}
</script>

<style scoped>
	.u-card{
		border-radius: 15px;
	}
	/deep/ .u-card__head{
		border-bottom: 1px dashed #7BBEF8;
		background-image: linear-gradient(#B7DCFC, #ffffff);
	}
	/deep/ .u-card__header-title-text{
		color: #3D4FF4;
		
	}
	/deep/ .u-card__header-extra-text{
		color: #3D4FF4;
		font-size: 14px;
	}
	/deep/ .u-card__body{
		padding: 0px;
	}
	.content-box-text{
		padding: 10px;
		font-size: 14px;
		color: #666666;
		line-height: 40px;
	}
	.postionIcon{
		width: 30px;
		height: 30px;
		vertical-align: middle;
	}
	.postion{
		width: 100%;
		border-bottom: 1px solid #DDDDDD;
		height: 50px;
	}
	.postionDesc{
		display: flex;
	}
	.desc1{
		flex: 1;
		/* width: 50%; */
		/* float: left; */
	}
	.textcenter{
		display:flex;
		align-items:center;
		justify-content:center;
		line-height: 40px;
	}
	.descBottom{
		width: 100%;
		height: 70px;
		background-color: #7F8BF6;
		
	}
	
	.sub{
		margin-top: 10px;
		width: 40%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #FFA237;
	}
	.descBottom view{
		color: #ffffff;
		padding-left: 20px;
		line-height: 32px;
		font-size: 14px;
	}
</style>
