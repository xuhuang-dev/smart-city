<template>
	<view class="wrap">
		<view class="line1">
			<view class="line">起点：{{bus.first}} -- 终点：{{bus.end}}</view>
		</view>
		<u-cell-group :border="false">
			<u-cell-item title="乘客手机号码" :arrow="false">
				<input type="u-tel" v-model="phone" maxlength="11" >
			</u-cell-item>
			<u-cell-item title="上车地点" :arrow="false">
				<picker @change="changeStart" :value="start" :range="stepList">
					<view>{{stepList[start]}}</view>
				</picker>
			</u-cell-item>
			<u-cell-item title="下车地点" :arrow="false">
				<picker @change="changeEnd" :value="end" :range="stepList">
					<view>{{stepList[end]}}</view>
				</picker>
			</u-cell-item>
			<u-cell-item title="出行日期" :value="selected" :arrow="false" @click="show=!show"></u-cell-item>
		</u-cell-group>
		<u-calendar v-model="show" @change="change" maxDate="2023-12-31"></u-calendar>
		<view style="padding: 40rpx;"><button type="primary" @click="next()">确认购买</button></view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				bus: {},
				data: {},
				stepList: [],
				startList: [],
				endList: [],
				name: "",
				phone: '',
				start: 0,
				end: 1,
				show: false,
				selected: null,
			}
		},
		onLoad(option) {
			this.data = JSON.parse(option.data)
			// 获取公交线路信息
			this.$u.get(api.GET_BUSLINE_INFO + '/' + this.data.id).then(res => {
				this.bus = res.data;
			})
			// 获取本条线路的站点信息
			this.$u.get(api.GET_BUSSTOP_LIST, {
				linesId: this.data.id
			}).then(res => {
				this.stepList = res.rows.map(item => {
					return item.name
				});
			})
		},
		methods: {
			change(data) {
				console.log(data)
				this.selected = data.result
			},
			changeStart(e) {
				this.start = e.target.value // 记录上车地点
			},
			changeEnd(e) {
				this.end = e.target.value // 记录下车地点
			},
			next() {
				if(this.phone.length==0||this.phone==null) {
					this.$u.toast("请输入手机号")
					return 
				}else if(this.selected==null) {
					this.$u.toast("请选择出行日期")
					return 
				}else if(this.end==this.start){
					this.$u.toast("下车地点不可为上车地点")
					return
				}else if(this.end<this.start){
					this.$u.toast("下车地点应在上车地点之后的站点")
					return
				}
				const data = {
					...this.data,
					// name: this.name,
					phone: this.phone,
					start: this.start,
					end: this.end,
					stepList: this.stepList,
					date:this.selected,
				}
				uni.navigateTo({
					url: `./step4?data=${JSON.stringify(data)}`
				})
			}
		}
	}
</script>

<style scoped>
	page {
		background-color: #fff;
	}

	.line1 {
		display: flex;
		margin-bottom: 30rpx;
	}

	.line {
		width: 100vw;
		height: 100rpx;
		line-height: 100rpx;
		font-size: 40rpx;
		border-bottom: 1px solid #ccc;
		text-align: center;
	}

	.ipt {
		flex: 1;
		border-bottom: 1px solid #ccc;
		padding: 10rpx 40rpx;
	}
</style>
