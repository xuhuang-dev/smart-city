<template>
	<view>
		<view class="i-box" isShadow>
			<view class="result">
				<text style="color: #6E78F7;"> 就 诊 人 </text>
				<text style="margin-left: 8px;">{{appointData.pName}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;">预约科室</text>
				<text>{{appointData.dptName}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;">门诊类型</text>
				<text>{{appointData.typeName}}</text>
			</view>
			<view class="result">
				<text style="color: #6E78F7;"> 金 额 </text>
				<text style="margin-left: 25px;">{{appointData.money}}元</text>
			</view>
		</view>
		<view class="i-box" isShadow>
			<u-cell-group :border="false">
				<u-cell-item link title="选择日期" :arrow="false" :border-bottom="false" :title-style="{color: '#6E78F7'}">
					<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
						<view class="uni-input">{{date}}</view>
					</picker>
				</u-cell-item>
				<u-cell-item link title="选择时间" :arrow="false" :border-bottom="false" :title-style="{color: '#6E78F7'}">
					<picker mode="time" :value="time" start="09:01" end="21:01" @change="bindTimeChange">
						<view class="uni-input">{{time}}</view>
					</picker>
				</u-cell-item>
			</u-cell-group>
		</view>
		<view class="below">
			<button class="clear" type="primary" @click="clear">取消</button>
			<button class="sub" type="primary" @click="sub">确定</button>
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
				title: 'picker',
				multiIndex: [0, 0, 0],
				date: getDate({
					format: true
				}),
				startDate: getDate('start'),
				endDate: getDate('end'),
				time: '12:01',
				appointData: {}
			}
		},
		onLoad() {
			uni.$on('appointInfo', (res) => {
				console.log(res.data)
				this.appointData = res.data;
			})
		},
		methods: {
			bindDateChange: function(e) {
				this.date = e.detail.value
			},
			bindTimeChange: function(e) {
				this.time = e.detail.value
			},
			clear() {
				uni.navigateBack({
					delta: 1
				})
			},
			sub() {
				if (this.date.length > 0 && this.time.length > 0) {
					this.$u.post(api.CREATE_APPOINT_ORDER, {
						patientName: this.appointData.pName,
						categoryId: this.appointData.did,
						type: this.appointData.typeId,
						money: this.appointData.money,
						startime: this.date + ' ' + this.time,
					}).then(res => {
						if (res.code == 200) {
							uni.showToast({
								title: '预约成功！',
								duration: 1000
							});

							setTimeout(function() {
								uni.navigateTo({
									url: 'appointmentResult'
								})
							}, 1000)
						}
					})
				} else {
					uni.showToast({
						title: '请选择时间！',
						icon: 'none'
					})
				}
			}

		}
	}
</script>

<style scoped>
	.result text {
		line-height: 40px;
		padding-left: 20px;
	}

	.sub {
		width: 35%;
		position: absolute;
		right: 10%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #6E78F7;
	}

	.below {
		width: 100%;
	}

	.clear {
		width: 35%;
		position: absolute;
		left: 10%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #FFFFFF;
		color: #6E78F7;
		border: 1px solid #6E78F7;
	}
</style>
