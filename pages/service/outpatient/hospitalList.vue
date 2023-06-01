<template>
	<view>
		<!-- <view class="example-body">
			<uni-search-bar radius="100" placeholder="请输入医院名称" @cancel="searchCancel" @confirm="searchData" />
		</view> -->
		<u-cell-group v-if="hospitalList.length>0">
			<!-- <u-cell-item icon="integral-fill" title="会员等级" value="新版本"></u-cell-item> -->
			<u-cell-item v-for="(item,index) in hospitalList" :key="index" link @click="clickCard(item.id)"
				:arrow="false" :title="item.hospitalName">
				<view slot="icon" class="slot-box">
					<image class="slot-image" :src="base+item.imgUrl" mode="widthFix"></image>
				</view>
				<u-rate :count="5" v-model="item.level" />
			</u-cell-item>
		</u-cell-group>
		<view v-else class="noData">未找到您要查询的医院！</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				base: api.SERVER_BASE,
				scrollT: 0,
				hospitalList: []
			}
		},
		created() {
			this.getDatalist('');
		},
		methods: {
			getDatalist(name) {
				if (name.length > 0) {
					this.$u.get(api.GET_HOSPITAL_LIST, {
						hospitalName: name
					}).then(res => {
						this.hospitalList = res.rows;
					})
				} else {
					this.$u.get(api.GET_HOSPITAL_LIST).then(res => {
						this.hospitalList = res.rows;
					})
				}
			},
			searchData(name) {
				this.getDatalist(name.value);
			},
			searchCancel() {
				this.hospitalList = [];
				this.getDatalist('');
			},
			clickCard(id) {
				uni.navigateTo({
					url: './hospitalDesc?id=' + id,
				});
			},
		} //,
		// onBackPress(e) {
		// 	uni.switchTab({
		// 		url: '../../index/index'
		// 	});
		// 	return true;
		// }
	}
</script>

<style>
	.slot-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;

	}

	.slot-image {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		margin-right: 10px;
		width: 50px;
		height: 50px;
	}

	.slot-text {
		flex: 1;
		font-size: 14px;
		margin-right: 10px;
	}

	.score {
		margin-top: 20px;
	}

	.noData {
		background-color: #fff;
		font-size: 16px;
		line-height: 40px;
		text-align: center;
		padding: 20px 0;
	}
</style>
