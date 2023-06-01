<template>
  <view>
    <view class="line1">
      <view class="line">乘车线路：{{bus.first}} -- {{bus.end}}</view>
      <view class="row">
        <view class="tip">手机号码：</view>{{order.phone}}
      </view>
      <view class="row">
        <view class="tip">上车地点：</view>{{order.stepList[order.start]}}
      </view>
      <view class="row">
        <view class="tip">下车地点：</view>{{order.stepList[order.end]}}
      </view>
      <view class="row">
        <view class="tip">乘车日期：</view>
        <view>
          {{order.date}} 
        </view>
      </view>
      <view style="padding: 40rpx 80rpx;"><button type="primary" @click="next()">提交订单</button></view>
    </view>
  </view>
</template>

<script>
  import api from '../../../common/apiUtil.js'
  export default {
    data() {
      return {
        bus: {},
        order: {}, // 订单信息
      }
    },
    onLoad(option) {
      this.order = JSON.parse(option.data)
      // 获取公交线路信息
      this.$u.get(api.GET_BUSLINE_INFO + '/' + this.order.id).then(res => {
        this.bus = res.data;
      })

    },
    methods: {
      next() {
        this.$u.post(api.ADD_BUS_ORDER, {
          start: this.order.stepList[this.order.start],
          end: this.order.stepList[this.order.end],
          price: this.bus.price,
          path: this.bus.name,
          status: 1
        }).then(res => {
          uni.showToast({
          	title: res.msg,
          	duration: 2000
          });
					setTimeout(()=>{
						this.$u.route({
							url:"pages/home/home",
							type:"tab"
						})
					},2000)
					
          // uni.reLaunch({
          // 	url: '../../index/index'
          // });
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
    margin-bottom: 30rpx;
  }

  .line {
    width: 100vw;
    height: 100rpx;
    line-height: 100rpx;
    font-size: 40rpx;
    border-bottom: 1px solid #ccc;
    text-align: center;
    font-weight: 700;
  }

  .row {
    display: flex;
    margin: 30rpx 0;
    align-items: center;
  }

  .tip {
    width: 400rpx;
    text-align: center;
  }
</style>
