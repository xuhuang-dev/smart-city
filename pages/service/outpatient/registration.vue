<template>
	<view>
		<view class="below">
			<button :class="typeNum==2?'clear active':'clear'" type="primary" @click="typeCheck(2)">普通号</button>
			<button :class="typeNum==1?'sub active':'sub'" type="primary" @click="typeCheck(1)">专家号</button>
		</view>
		<view class="departWrap">
			<view v-if="catelist.length>0">
				<u-cell-group :border="false" v-for="(item,index) in catelist" :key="index">
					<u-cell-item showArrow :title="item.categoryName" clickable
						@click="confirms(item.categoryName,item.money,item.id)" />
				</u-cell-group>

			</view>
			<view class="noData" v-else>暂无科室</view>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				catelist: [],
				typeNum: 1,
				pName: ''
			}
		},
		onLoad(options) {
			this.pName = options.name;
		},
		created() {
			this.getDeparts(this.typeNum);
		},
		methods: {
			getDeparts(id) {
				this.$u.get(api.GET_DEPART_LIST + '?type=' + id).then(res => {
					this.catelist = res.rows;
				})
			},
			confirms(cName, money, did) {
				let that = this;
				let typeName = this.typeNum == 1 ? '专家号' : '普通号';
				setTimeout(function() {
					uni.$emit('appointInfo', {
						data: {
							typeName: typeName,
							dptName: cName,
							pName: that.pName,
							money: money,
							did: did,
							typeId: that.typeNum
						},
					})
				}, 300)

				uni.navigateTo({
					url: './appointmentConfirm'
				});
			},
			typeCheck(num) {
				this.typeNum = num;
				this.getDeparts(num);
			}
		}
	}
</script>

<style>
	.below {
		width: 100%;
		margin-top: 20px;
	}

	.below::after {
		content: '';
		display: block;
		clear: both;
	}

	.sub {
		width: 35%;
		border-radius: 20px;
		font-size: 16px;
		background-color: #FFFFFF;
		color: #6E78F7;
		border: 1px solid #6E78F7;
		float: left;
		margin-left: 10%;
	}

	.clear {
		width: 35%;
		float: right;
		border-radius: 20px;
		font-size: 16px;
		background-color: #FFFFFF;
		color: #6E78F7;
		border: 1px solid #6E78F7;
		margin-right: 10%;

	}

	.departWrap {
		margin-top: 20px;
	}

	.uni-list {
		clear: both;
	}

	.active {
		color: #fff;
		background-color: #39D577;
		border: none;
		border-radius: 20px;
	}

	.noData {
		width: 100%;
		background: #fff;
		font-size: 18px;
		color: #3B4144;
		text-align: center;
		padding: 50px 0 30px 0;
	}
</style>
