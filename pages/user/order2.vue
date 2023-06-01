<template>
	<view>
		<!-- <view class="head">
			<view :class="['menu', cIndex==0?'menu-active':'']" @click="cIndex=0">待支付</view>
			<view :class="['menu', cIndex==1?'menu-active':'']" @click="cIndex=1">已支付</view>
		</view> -->

		<view v-for="item in orders">
			<view v-if="cIndex==item.status">
				<view v-if="item.orderType == 'movie'"></view>
				<view v-else-if="item.orderType == 'tabkeout'"></view>
				<view v-else class="card">
					<view class="line">{{item.start}} -> {{item.end}}</view>
					<view class="row">订单类型：巴士订单</view>
					<view class="row">订单编号：{{item.orderNum}}</view>
					<view class="row">订单日期：{{item.createTime}}</view>
					<view class="row">
						<view class="tip">票价：</view>{{item.price}} 元
					</view>
					<view class="row">
						<view class="tip">乘客姓名：</view>{{item.userName}}
					</view>
					<view class="row">
						<view class="tip">手机号码：</view>{{item.userTel}}
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import api from '../../common/apiUtil.js'
	export default {
		data() {
			return {
				cIndex: 0,
				orders: []
			}
		},
		onLoad(option) {
			this.id = option.id;

		},
		onShow() {
			console.log("getBusOrder")
			this.getBusOrder()
		},
		methods: {
			getBusOrder() {
				// 巴士订单
				this.$u.get(api.GET_ORDER_LIST).then(res => {
					this.orders = res.rows;
					console.log(res)
					// 外卖和电影订单
					// this.$u.get(api.baseUrl + '/prod-api/api/allorder/list').then(res1 => {
					//   this.orders.concat(res1.rows);
					// })
				})
			}
		}
	}
</script>

<style scoped>
	/*  page {
    background: #fff;
  } */

	.head {
		display: flex;
	}

	.menu {
		flex: 1;
		height: 80rpx;
		line-height: 80rpx;
		text-align: center;
		border-bottom: 1px solid #ccc;
	}

	.menu-active {
		background: #12d357;
		color: #fff;
		border-bottom: 1px solid #12d357;
	}

	.row {
		display: flex;
		margin: 20rpx 0;
		align-items: center;
	}

	.line {
		font-size: 40rpx;
		text-align: center;
		color: #007AFF;
		font-weight: 700;
	}

	.tip {
		width: 200rpx;
		/* text-align: center; */
	}

	.card {
		padding: 20rpx 40rpx;
		border-bottom: 1px solid #ccc;
		background: #fff;
	}
</style>
