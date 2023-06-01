<template>
  <view class="wrap">
    <view class="line1">
      <view class="line">乘车线路：{{bus.first}} -- {{bus.end}}</view>
    </view>
		<u-cell-group :border="false">
				<u-cell-item title="运营时间" :value="runTime" :arrow="false"></u-cell-item>
				<u-cell-item title="票价" :value="bus.price" :arrow="false"></u-cell-item>
				<u-cell-item title="里程" :value="bus.mileage+'km'" :arrow="false"></u-cell-item>
			</u-cell-group>
    <button type="primary" @click="next()">购买车票</button>
		
  </view>
</template>

<script>
  import api from '../../../common/apiUtil.js'
  export default {
    data() {
      return {
        bus: {},
        id: 0,
				selected:'',
				show:false,
      }
    },
		computed:{
			runTime(){
				return this.bus.startTime+"--"+this.bus.endTime
			},
		},
    onLoad(option) {
      this.id = option.id;
      this.$u.get(api.GET_BUSLINE_INFO + '/' + option.id).then(res => {
        this.bus = res.data;
      })
    },
    methods: {
			change(data) {
				console.log(data)
			  this.selected = data.result
			},
			next() {
			  const data = {
			    id: this.id,
			  }
			  uni.navigateTo({
			    url: `./step3?data=${JSON.stringify(data)}`
			  })
			}
    }
  }
</script>

<style scoped>
  .wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  page {
    background-color: #fff;
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
    margin-bottom: 30rpx;
  }

  .line2 {
    display: flex;
    font-size: 40rpx;
    margin-bottom: 30rpx;
  }

  .icon {
    width: 40rpx;
    height: 40rpx;
    margin-right: 10rpx;
  }

  .line {
    width: 100vw;
    height: 100rpx;
    line-height: 100rpx;
    font-size: 40rpx;
    border-bottom: 1px solid #ccc;
    text-align: center;
  }

  .price {
    font-size: 40rpx;
    margin-bottom: 30rpx;
  }

  .tip {
    display: inline-block;
    width: 100rpx;
    margin-right: 20rpx;
  }
</style>
