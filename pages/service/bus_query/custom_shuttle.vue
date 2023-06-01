<template>
	<view>
		<view v-for="(item,index) in busList">
			<view class="i-box" @click="go(item.id)">
				<view class="title">{{item.name}}</view>
				<view class="content">
					<view class="u-flex u-row-between">
						<view class="">起始站 {{item.first}}</view>
						<view class="">运营时间：{{item.startTime}} -- {{item.endTime}}</view>
					</view>
					<view class="u-flex u-row-between">
						<view class="">终点站 {{item.end}}</view>
						<view class="">票价：{{item.price}} 里程：{{item.mileage}}km</view>
					</view>
					<view class="" style="overflow-x:scroll;width: 100%;white-space: nowrap;">
						<text class="u-type-primary" v-for="(item2,index2) in busListStop[index].stopList">
							{{index2<(busListStop[index].stopList.length-1)?item2.name+'-->':item2.name}}
						</text>
					</view>
				</view>
			</view>
			<!-- <view style="padding: 20rpx 60rpx;" v-show="item.id == currentId">
				<view v-for="(item, index) in stepList">
					<text v-if="index==0" class="tip">起点：</text>
					<text v-else-if="index==stepList.length-1" class="tip">终点：</text>
					<text v-else class="tip"></text>
					<text>{{item.name}}</text>
				</view>
			</view> -->
		</view>

	</view>
</template>

<script>
	import {
		onBeforeMount
	} from "vue";
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				busList: [],
				busListStop: [],
			}
		},
		created() {
			this.getBusListAndStop();
		},
		methods: {
			getStopList(id) {
				return this.$u.get(api.GET_BUSSTOP_LIST, {
					linesId: id
				})
			},
			// getBusList() {
			// 	this.$u.get(api.GET_BUS_ROUTES)
			// 	.then(res => {
			// 		this.busList = res.rows;
			// 	})
			// },
			async getBusListAndStop() {
				await this.$u.get(api.GET_BUS_ROUTES).then(res => {
					this.busList = res.rows;
				})
				for (var i = 0; i < this.busList.length; i++) {
					await this.$u.get(api.GET_BUSSTOP_LIST, {
						linesId: this.busList[i].id
					}).then(res => {
						this.busListStop.push({
							id: this.busList.id,
							stopList: res.rows
						})
					})
				}
				console.log("busListStop", this.busListStop)
			},
			onClick(id) {
				// if (this.currentId == id) {
				// 	this.currentId = 0;
				// } else {
				// 	// 获取巴士的站点
				// 	this.$u.get(api.GET_BUSSTOP_LIST, {
				// 		linesId: id
				// 	}).then(res => {
				// 		this.stepList = res.rows;
				// 		this.currentId = id
				// 	})
				// }
			},
			go(id) {
				uni.navigateTo({
					url: './step1?id=' + id
				})
			}
		}
	}
</script>

<style scoped>
	.card {
		display: flex;
		padding: 20rpx;
		border-bottom: 1px solid #ccc;
		background: #fff;
	}

	.left {
		width: 160rpx;
	}

	.name {
		width: fit-content;
		height: 40rpx;
		padding: 10rpx;
		background: red;
		color: #fff;
		margin-right: 20rpx;
		border-radius: 10rpx;
	}

	.line1 {
		display: flex;
	}

	.line2 {
		display: flex;
	}

	.icon {
		width: 40rpx;
		height: 40rpx;
		margin-right: 10rpx;
	}

	.line {
		font-size: 40rpx;
		color: #00a400
	}

	.price {
		margin-right: 40rpx;
	}

	.tip {
		display: inline-block;
		width: 100rpx;
		margin-right: 20rpx;
	}
</style>
