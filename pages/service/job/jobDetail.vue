<template>
	<view class="jobDetail">
		<view class="jobSection">
			<view class="title">工作列表</view>
			<view class="worklist">
				<view class="workItem">
					<view class="itemRow">
						<view class="label">职位名称：</view>
						<view class="text">{{details.name}}</view>
					</view>
					<view class="itemRow">
						<view class="label">职位描述：</view>
						<view class="text">{{details.need}}</view>
					</view>
					<view class="itemRow">
						<view class="label">工作地址：</view>
						<view class="text">{{details.address}}</view>
					</view>
					<view class="itemRow">
						<view class="label">薪&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;酬：</view>
						<view class="text">{{details.salary}}</view>
					</view>
					<view class="itemRow">
						<view class="label">联&nbsp;&nbsp;系&nbsp;&nbsp;人：</view>
						<view class="text">{{details.contacts}}</view>
					</view>
					<view class="itemRow">
						<view class="label">工作描述：</view>
						<view class="text">{{details.obligation}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="jobSection">
			<view class="title">公司信息</view>
			<view class="worklist">
				<view class="workItem">
					<view class="itemRow">
						<view class="label">公司名称：</view>
						<view class="text">{{companyInfo.companyName}}</view>
					</view>
					<view class="itemRow">
						<view class="label">公司简介：</view>
						<view class="text">{{companyInfo.introductory}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="participate">
			<button type="primary" @click="resume">投简历</button>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js';
	export default {
		data() {
			return {
				details: {},
				companyInfo: {},
				cid: 0,
				pid: 0,
				timer: null
			}
		},
		onLoad(options) {
			this.getDetails(options.id);
			this.pid = options.id;
		},
		methods:{
			getDetails(id) {
				this.$u.get(api.GET_POST_DETAIL+id).then(res => {
					if(res.code == 200) {
						this.details = res.data;
						this.getCompanyDetails(res.data.companyId);
						this.cid = res.data.companyId;
					}
				})
			},
			getCompanyDetails(id) {
				this.$u.get(api.GET_COMPANY_DETAIL+id).then(res => {
					if(res.code == 200) {
						this.companyInfo = res.data;
					}
				})
			},
			resume() {
				clearTimeout(this.timer)
				this.$u.post(api.DELIVER_RESUME,{
					companyId: this.cid,
					money: this.details.salary,
					postId: parseInt(this.pid),
					postName: this.details.name
				}).then(res => {
					if(res.code == 200) {
						uni.showToast({
							icon: 'success',
							title: res.msg
						});
						this.timer = setTimeout(function() {
							uni.navigateBack();
						},1500)
					} else {
						uni.showToast({
							title: res.msg
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.jobDetail{
		padding-bottom: 60px;
		.jobSection{
			.title{
				width: 100%;
				background-color: #d5d5d5;
				font-size: 16px;
				line-height: 30px;
				color: #333;
				text-indent: 1em;
			}
			.worklist{
				box-sizing: border-box;
				padding: 0 15px;
				.workItem{
					padding: 10px 0;
					.itemRow{
						display: flex;
						font-size: 16px;
						line-height: 24px;
						color: #666;
						.label{
							width: 80px;
							white-space: nowrap;
						}
						.text{
							flex: auto;
						}
					}
				}
			}
		}
		.participate{
			width: 100%;
			background-color: #fff;
			padding: 10px 30px;
			border-top: 1px #e6e6e6 solid;
			position: fixed;
			left: 0;
			bottom: 0;
			z-index: 9;
			button{
				background-color: #007AFF;
				color: #fff;
				height: 40px;
				line-height: 40px;
				font-size: 16px;
			}
		}
	}
</style>
