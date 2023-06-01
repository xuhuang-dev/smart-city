<template>
  <view class="u-p-20">
		<view class="">
			<view style="padding: 20rpx 0">出行日期：</view>
			<input type="text" v-model="selected">
		</view>
		<button type="primary" @click="show=!show">选择出行日期</button>
		<u-calendar  v-model="show" @change="change" maxDate="2023-12-31"></u-calendar>
    <!-- <uni-calendar :insert="true" :lunar="true" :selected="selected" @change="change" /> -->
    <button type="primary" @click="next()">下一步</button>
  </view>
</template>

<script>
  import api from '../../../common/apiUtil.js'
  export default {
    data() {
      return {
				show:false,
        bus: {},
        id: 0,
        selected: ''
      }
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
          date: this.selected
        }
        uni.navigateTo({
          url: `./step3?data=${JSON.stringify(data)}`
        })
      }
    }
  }
</script>

<style scoped>
  page {
    background-color: #fff;
  }
	.title{
			color: $u-type-primary;
			text-align: center;
			padding: 20rpx 0 0 0;
		}
</style>
