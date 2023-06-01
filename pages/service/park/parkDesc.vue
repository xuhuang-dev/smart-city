<template>
	<view>
		<view class="example-body">
			<i-box class="u-m-t-20">
				<view class="u-text-center u-m-b-20 u-font-30 " style="font-weight: 600;">请选择出入时间进行</view>
				<u-cell-group :border="true">
					<u-cell-item :arrow="false">
						<view slot="title" class="u-flex u-font-30" style="color: #6E78F7;">
							入场时间
							<view style="position: absolute;right: 60px;">
								<picker mode="date" :value="indate" :start="startDate" :end="endDate"
									@change="inbindDateChange">
									<view class="uni-input">{{indate}}</view>
								</picker>
							</view>
							<view style="position: absolute;right: 0;">
								<picker mode="time" :value="intime" start="09:01" end="21:01"
									@change="inbindTimeChange">
									<view class="uni-input">{{intime}}</view>
								</picker>
							</view>
						</view>
					</u-cell-item>
					<u-cell-item :arrow="false">
						<view slot="title" class="u-flex u-font-30" style="color: #6E78F7;">
							出场时间
							<view style="position: absolute;right: 60px;">
								<picker mode="date" :value="godate" :start="startDate" :end="endDate"
									@change="gobindDateChange">
									<view class="uni-input">{{godate}}</view>
								</picker>
							</view>
							<view style="position: absolute;right: 0;">
								<picker mode="time" :value="gotime" start="09:01:00" end="21:01:00"
									@change="gobindTimeChange">
									<view class="uni-input">{{gotime}}</view>
								</picker>
							</view>
						</view>
					</u-cell-item>
				</u-cell-group>
				<button class="sub" type="primary" @click="search">
					<image src="/static/icons/search.png" class="searchImg"></image>
				</button>
			</i-box>
			<view v-if="parkList.length>0">
				<u-card v-for="(item,index) in parkList" :key="index" :title="item.plateNumber" title-color="#6E78F7"
					:sub-title="'收费金额：'+item.monetary" sub-title-color="orange" isShadow>
					<view class="" slot="body">
						<view class="parkDesc">
							<text style="color: #707070;">入场时间</text>
							<text>{{item.entryTime}}</text>
						</view>
						<view class="parkDesc">
							<text style="color: #707070;">出场时间</text>
							<text>{{item.outTime}}</text>
						</view>
						<view class="parkDesc">
							<text style="color: #707070;">停车场名称</text>
							<text>{{item.parkName}}</text>
						</view>
					</view>
				</u-card>
			</view>
			<view class="illNoData" v-else>
				<text>暂无停车记录！</text>
			</view>
			<button v-show="isShow" class="sub" type="primary" @click="more">查看更多</button>
		</view>
	</view>
</template>

<script>
	function getDate(type) {
		const date = new Date();

		let year = date.getFullYear();
		let month = date.getMonth() + 1;
		let day = date.getDate();

		if (type === 'start') {
			year = year - 60;
		} else if (type === 'end') {
			year = year + 2;
		}
		month = month > 9 ? month : '0' + month;;
		day = day > 9 ? day : '0' + day;

		return `${year}-${month}-${day}`;
	}
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				parkList: [],
				isShow: true,
				startDate: getDate('start'),
				endDate: getDate('end'),
				indate: '日期',
				godate: '日期',
				intime: '时间',
				gotime: '时间',
				pageNum: 1
			}
		},
		created() {
			this.getRecordlist(1);
		},
		methods: {
			getRecordlist(num) {
				this.$u.get(api.GET_PARK_RECORD, {
					pageNum: num,
					pageSize: 3
				}).then(res => {
					console.log(res)
					res.rows.map((item) => {
						this.parkList.push(item)
					});

					if (this.pageNum > 1 && res.rows.length == 0) {
						uni.showToast({
							title: '没有更多了',
							icon: 'none'
						});
						this.isShow = false;
					}
				})
			},
			getRecdByTime(entry, out) {
				this.$u.get(api.GET_PARK_RECORD + '?entryTime=' + entry + '&outTime=' + out).then(res => {
					this.parkList = res.rows;
					this.isShow = false;
				})
			},
			inbindDateChange: function(e) {
				this.indate = e.detail.value
			},
			gobindDateChange: function(e) {
				this.godate = e.detail.value
			},
			inbindTimeChange: function(e) {
				this.intime = e.detail.value
			},
			gobindTimeChange: function(e) {
				this.gotime = e.detail.value
			},
			search() {
				let inTime = this.indate + ' ' + this.intime + ':00';
				let goTime = this.godate + ' ' + this.gotime + ':00';
				if (inTime.length < 14 || goTime.length < 14) {
					uni.showToast({
						title: '请选择完整时间段',
						icon: 'none'
					})
				} else {
					this.getRecdByTime(inTime, goTime);
				}
			},
			more() {
				this.pageNum += 1;
				this.getRecordlist(this.pageNum);
			}
		}
	}
</script>

<style scoped>
	.example-body {
		padding-bottom: 20px;
		/* padding-top: 20px; */
	}

	.title {
		line-height: 40px;
		text-indent: 1em;
		color: #333333;
		font-weight: 600;
	}

	.uni-card {
		border-radius: 10px;
	}

	.parkTime {
		width: calc(100% - 10px);
		height: 40px;
		margin: 5px auto;
		background-color: #FBFBFB;
		border: 1px solid #D6D6D6;
		border-radius: 10px;
	}

	.parkTime text {
		line-height: 40px;
		padding-left: 20px;
	}

	button.sub {
		margin-top: 10px;
		width: 40%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #FFA237;
	}

	.searchImg {
		width: 30px;
		height: 30px;
		vertical-align: middle;
	}

	/deep/ .uni-card__header {
		border-bottom: 1px dashed #7BBEF8;
		background-image: linear-gradient(#B7DCFC, #ffffff);
	}

	/deep/ .uni-card__header-title-text {
		color: #3D4FF4;

	}

	/deep/ .uni-card__header-extra-text {
		color: #FFA237;
		font-size: 14px;
	}

	.parkDesc text {
		line-height: 30px;
		padding-left: 20px;
	}

	.illNoData {
		width: calc(100% - 30px);
		margin: 20px auto;
		padding: 50px 0;
		background-color: #fff;
		border-radius: 8px;
		box-shadow: 0px 0px 5px 1px rgba(0, 0, 0, 0.1);
		text-align: center;
	}
</style>
