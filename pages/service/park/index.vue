<template>
	<view>
		<view class="example-body">
			<view v-if="parkList.length>0">
				<u-card v-for="(item,index) in parkList" :key="index" :title="item.parkName" :subTitle="item.distance+'公里'" isShadow  @click="clickCard(item.id)" title-color="#3D4FF4" subTitleColor="#3D4FF4" :subTitleSize="28">
					<!-- <view slot="head" style="" > -->
						<!-- <text>{{item.parkName}}</text> -->
					<!-- </view> -->
					<view slot="body">
						<text class="content-box-text">空位{{item.vacancy}}个&nbsp;&nbsp;|&nbsp;&nbsp;停车费{{item.rates}}元/小时\n</text>
						<text class="content-box-text">{{item.address}}</text>
					</view>
					<image src="../../../static/icons/right.png" class="rightIcon"></image>
				</u-card>
			</view>
			<view class="illNoData" v-else>
				<text>附近暂无停车场！</text>
			</view>
			<view class="btnFlex">
				<button type="primary" class="sub" @click="toRecord">查看停车记录</button>
				<button v-show="parkList.length>2" class="sub" type="primary" @click="clickSub">查看更多</button>
			</view>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				parkList: [],
				pageNum: 1
			}
		},
		created() {
			this.getParklist(1);
		},
		methods: {
			getParklist(num) {
				this.$u.get(api.GET_PARK_LIST,{
					pageNum: num,
					pageSize: 3
				}).then(res => {
					console.log(res)
					res.rows.map((item) => {
						this.parkList.push(item);
					});
					
					if(this.pageNum>1 && res.rows.length==0){
						uni.showToast({
							title: '没有更多了',
							icon: 'none'
						})
					}
				})
			},
			toRecord() {
				uni.navigateTo({
					url: 'parkDesc'
				})
			},
			clickCard(id) {
				uni.navigateTo({
				    url: './park?id='+id
				});
			},
			clickSub(){
				this.pageNum+=1;
				this.getParklist(this.pageNum);
			}
		}
	}
</script>

<style scoped lang="scss">
	.example-body{
		padding-bottom: 20px;
	}
	
	
	.content-box-text{
		padding: 10px;
		font-size: 14px;
		color: #666666;
		line-height: 40px;
	}
	.rightIcon{
		width: 30px;
		height: 30px;
		margin-left: 10px;
		position: absolute;
		right: 20px;
		top: 55%;
	}
	.btnFlex{
		width: 100%;
		display: flex;
		justify-content: center;
	}
	.sub{
		margin-top: 10px;
		width: 40%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #FFA237;
	}
	.illNoData{
		width: calc(100% - 30px);
		margin: 20px auto;
		padding: 50px 0;
		background-color: #fff;
		border-radius: 8px;
		box-shadow: 0px 0px 5px 1px rgba(0, 0, 0, 0.1);
		text-align: center;
	}
</style>
<style lang="scss">
	.u-card{
		border-radius: 15px;
		
	}
	/deep/ .u-card__head{
		border-bottom: 1px dashed #7BBEF8;
		background-image: linear-gradient(#B7DCFC, #ffffff);
	}
	/deep/ .u-card__head--left__title{
		color: #3D4FF4;
	};
	/deep/ .u-card__head__title__text{
		color: #3D4FF4;
		font-size: 14px;
	}
</style>