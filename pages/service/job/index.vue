<template>
	<view class="jobContainer">
		<view class="jobTabs">
			<view :class="tabIndex==0?'tabItem active':'tabItem'" @click="tabtap(0)">找工作</view>
			<view :class="tabIndex==1?'tabItem active':'tabItem'" @click="tabtap(1)">投递记录</view>
			<view :class="tabIndex==2?'tabItem active':'tabItem'" @click="tabtap(2)">个人简历</view>
		</view>
		<view class="swiperContent">
			<swiper @change="swiperChange" :current="tabIndex" class="jobSwiper">
				<swiper-item class="jobHunting">
					<scroll-view class="scrollView" scroll-y="true">
						<u-search input-align="center" :animation="true" bg-color="white" shape="square" @custom="searchJob" @search="searchJob" :clearabled="false" v-model="searchText" placeholder="搜索"></u-search>
						
						<view class="jobSection">
							<view class="title">热门职位</view>
							<view class="postlist">
								<scroll-view class="scroll-view_H" scroll-x="true" :scroll-left="scrollL" scroll-with-animation="true">
									<view class="scroll-view-item_H" v-for="(item,index) in postlist" :key="index" @click="checkPost(index,item.id,$event)">
										<text :class="index==postActive?'scItemActive textcenter':'textcenter'">{{item.professionName}}</text>
									</view>
								</scroll-view>
							</view>
						</view>
						<view class="jobSection">
							<view class="title">工作列表</view>
							<view class="worklist">
								<view class="workItem" v-for="item in worklist" @click="navToJobDetail(item.id)">
									<view class="itemRow">
										<view class="label">职位名称：</view>
										<view class="text">{{item.professionName}}</view>
									</view>
									<view class="itemRow">
										<view class="label">职位描述：</view>
										<view class="text">{{item.obligation}}</view>
									</view>
									<view class="itemRow">
										<view class="label">单位地址：</view>
										<view class="text">{{item.address}}</view>
									</view>
									<view class="itemRow">
										<view class="label">薪&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;酬：</view>
										<view class="text">{{item.salary}}</view>
									</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</swiper-item>
				<swiper-item class="deliverHistory">
					<scroll-view class="scrollView" scroll-y="true">
						<view class="jobSection">
							<view class="worklist">
								<view class="workItem" v-for="item in deliverHistory">
									<view class="itemRow">
										<view class="label">职位名称：</view>
										<view class="text">{{item.postName}}</view>
									</view>
									<view class="itemRow">
										<view class="label">公司名称：</view>
										<view class="text">{{item.companyName}}</view>
									</view>
									<view class="itemRow">
										<view class="label">薪&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;酬：</view>
										<view class="text">{{item.money}}</view>
									</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</swiper-item>
				<swiper-item class="resume">
					<scroll-view class="scrollView" scroll-y="true">
						<view class="personal">
							<view class="userHead">
								<view class="avatar">
									<u-icon class="avatarIcon" style="color: #fff;" type="person-filled" size="70"></u-icon>
								</view>
							</view>
							<view class="jobSection">
								<view class="worklist">
									<view class="workItem">
										<view class="itemRow">
											<view class="label">昵称：</view>
											<view class="text">{{userInfo.nickName}}</view>
										</view>
										<view class="itemRow">
											<view class="label">性别：</view>
											<view class="text">{{userInfo.sex=='0'?'男':'女'}}</view>
										</view>
										<view class="itemRow">
											<view class="label">Email：</view>
											<view class="text">{{userInfo.email}}</view>
										</view>
										<view class="itemRow">
											<view class="label">电话号码：</view>
											<view class="text">{{userInfo.phonenumber}}</view>
										</view>
									</view>
								</view>
							</view>
							<view class="manage">
								<button type="primary" @click="gotoManage">个人简历管理</button>
							</view>
						</view>
					</scroll-view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				tabIndex: 0,
				postlist: [],
				worklist: [],
				postActive:null,
				scrollL: 0,
				deliverHistory: [],
				userInfo: {},
				searchText:'',
			}
		},
		created() {
			this.getPostlist();
			this.getWorklist();
		},
		methods:{
			getPostlist() {
				this.$u.get(api.GET_PROFESSION_LIST).then(res => {
					if(res.code == 200){
						this.postlist = res.rows;
					}
				})
			},
			getWorklist(id) {
				this.$u.get(api.GET_POST_LIST,{
					professionId: id
				}).then(res => {
					if(res.code == 200){
						this.worklist = [];
						res.rows.forEach(item => {
							if(item.professionName != null) {
								this.worklist.push(item)
							}
						})
					}
				})
			},
			tabtap(index){
				this.tabIndex = index;
				if(index == 1) {
					this.getDeliverHistory();
				} else if(index == 2) {
					this.getUserInfo();
				}
			},
			swiperChange(e) {
				this.tabtap(e.detail.current);
			},
			searchJob() {
				this.worklist = this.worklist.filter((item) => {
					return item.professionName.indexOf(this.searchText) >= 0;
				})
			},
			checkPost(index,id,e) {
				const query = uni.createSelectorQuery().in(this);
				query.select('.scroll-view-item_H').boundingClientRect(data => {
						this.scrollL = e.currentTarget.offsetLeft-data.width;
				}).exec();
				this.postActive = index;
				this.getWorklist(id);
			},
			navToJobDetail(id) {
				uni.navigateTo({
					url: 'jobDetail?id='+id
				})
			},
			getDeliverHistory() {
				let userId = uni.getStorageSync('info').userId;
				this.$u.get(api.GET_DELIVER_LIST,{
					userId: userId
				}).then(res => {
					if(res.code == 200) {
						this.deliverHistory = res.rows;
					}
				})
			},
			getUserInfo() {
				this.$u.get(api.GET_USER_INFO).then(res => {
					this.userInfo = res.user;
				})
			},
			gotoManage() {
				let that = this;
				uni.navigateTo({
					url: 'resumeManage?id='+that.userInfo.userId
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.jobTabs{
		width: 100%;
		background-color: #fff;
		display: flex;
		justify-content: center;
		position: fixed;
		left: 0;
		/* #ifdef H5 */
		top: 44px;
		/* #endif */
		/* #ifdef APP-PLUS */
		top: 0;
		/* #endif */
		box-shadow: 0 1px 5px 1px #d3d3d3;
		z-index: 99;
		.tabItem{
			font-size: 16px;
			line-height: 44px;
			color: #666;
			padding: 0 15px;
			&.active{
				border-bottom: 2px #05682c solid;
				color: #05682c;
			}
		}
	}
	.swiperContent{
		padding-top: 46px;
		.jobSwiper{
			height: calc(100vh - 46px);
			.jobHunting{
				background-color: #fff;
				.scrollView{
					height: calc(100vh - 46px);
					.jobSection{
						// padding-top: 10px;
						.title{
							width: 100%;
							background-color: #007AFF;
							font-size: 16px;
							line-height: 30px;
							color: #fff;
							text-indent: 1em;
						}
						.postlist{
							.textcenter{
								text-align: center;
								font-size: 14px;
								line-height: 24px;
							}
							.scroll-view_H {
								/* 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 */
								white-space: nowrap;
								width: 100%;
							}
							.scroll-view_H::-webkit-scrollbar{
								display: none !important;
							}
							.scroll-view-item_H {
								display: inline-block;
								padding: 0 40upx;
								margin: 10px 0;
								border-right: 1px #d3d3d3 solid;
							}
							.scroll-view-item_H view{
								box-sizing: border-box;
							}
							.scItemActive{
								color: red;
							}
						}
						.worklist{
							box-sizing: border-box;
							padding: 0 15px;
							.workItem{
								padding: 10px 0;
								border-bottom: 1px #e6e6e6 solid;
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
				}
			}
			.deliverHistory{
				background-color: #fff;
				.scrollView{
					height: calc(100vh - 46px);
					.jobSection{
						// padding-top: 10px;
						.title{
							width: 100%;
							background-color: #007AFF;
							font-size: 16px;
							line-height: 30px;
							color: #fff;
							text-indent: 1em;
						}
						.worklist{
							box-sizing: border-box;
							padding: 0 15px;
							.workItem{
								padding: 10px 0;
								border-bottom: 1px #e6e6e6 solid;
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
				}
			}
			.resume{
				.scrollView{
					height: calc(100vh - 46px);
				}
				.personal{
					width: 100%;
					position: absolute;
					left: 0; top: 0; bottom: 0;
					background-color: #fff;
				}
				.userHead{
					padding: 25px 0;
					text-align: center;
				}
				.avatar{
					width: 80px;
					height: 80px;
					margin: 0 auto;
					border: 1px #ddd solid;
					border-radius: 50%;
					overflow: hidden;
					text-align: center;
					background-color: #a4d0ff;
					.avatarIcon{
						line-height: 80px;
					}
				}
				.manage{
					width: calc(100% - 80px);
					margin: 20px auto;
				}
				.jobSection{
					// padding-top: 10px;
					.title{
						width: 100%;
						background-color: #007AFF;
						font-size: 16px;
						line-height: 30px;
						color: #fff;
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
								color: #333;
								.label{
									flex: 1;
									white-space: nowrap;
									text-align: right;
								}
								.text{
									flex: 1;
								}
							}
						}
					}
				}
			}
		}
	}
</style>
