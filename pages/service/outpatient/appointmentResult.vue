<template>
	<view class="appointWrap">
		<view class="i-box" isShadow v-for="(item,index) in aptlist" :key="index">
			<view class="result">
				<text style="color: #6E78F7;">预约订单</text>
				<text style="margin-left: 8px;">{{item.orderNo}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;">预约金额</text>
				<text style="margin-left: 8px;">{{item.money}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;"> 就 诊 人 </text>
				<text style="margin-left: 8px;">{{item.patientName}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;">预约科室</text>
				<text>{{item.typesId==1?'专家号':'普通号'}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;">门诊类型</text>
				<text>{{item.categoryName}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;">预约时间</text>
				<text>{{item.reserveTime}}</text>
			</view>
		</view>
		<view class="" style="display: flex;align-items: center;margin: 20rpx 20rpx 0 20rpx;">
			<u-button class="sub u-flex-1" type="primary" shape="circle" size="medium" @click="goBack">返回首页</u-button>
			<u-button class="sub u-flex-1" shape="circle" type="primary" size="medium" @click="lookMore">查看更多</u-button>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				aptlist: [],
				pageNum: 0,
				pageSize: 3,
			}
		},
		created() {
			this.getlist();
		},
		methods: {
			lookMore() {
				this.pageNum++
				this.getlist()
			},
			getlist() {
				let user = uni.getStorageSync('info');
				this.$u.get(api.GET_APPOINT_LIST, {
					userId: user.userId,
					pageNum: this.pageNum,
					pageSize: this.pageSize,
				}).then(res => {
					console.log("appointmentResult-getlist", res)
					res.rows.forEach(item => {
						this.aptlist.push(item)
					})
				})
			},
			goBack() {
				this.$u.route({
					url: 'pages/home/home',
					type: 'tab'
				})
			},
			onBackPress(e) {
				uni.navigateTo({
					url: 'hospitalList'
				})
				return true;
			}

		}
	}
</script>

<style>
	.appointWrap {
		padding-bottom: 20px;
	}

	.result text {
		line-height: 40px;
		padding-left: 20px;
	}

	.sub {
		/* margin-top: 20px; */
		/* width: 90%; */
		/* border-radius: 20px; */
		font-size: 16px;
		background-color: #6E78F7;
	}
</style>
