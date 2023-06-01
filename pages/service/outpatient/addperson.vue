<template>
	<view class="settingInfo">
		<u-cell-group :border="false">
			<u-cell-item title="姓名" :arrow="false">
				<input class="uni-input nickName" maxlength="4" v-model="nickName" placeholder="请输入姓名" />
			</u-cell-item>
			<u-cell-item title="性别" :arrow="false">
				<view>
					<radio-group @change="radioChange">
						<label class="radio" v-for="(item, index) in sexlist" :key="item.value">
							<radio style="transform:scale(0.7)" :value="item.value" :checked="index === current" />
							{{item.name}}
						</label>
					</radio-group>
				</view>
			</u-cell-item>
			<u-cell-item title="身份证号" :arrow="false">
				<input class="uni-input IdNumber" v-model="IdNum" placeholder="请输入身份证号" />
			</u-cell-item>
			<u-cell-item title="出生年月" :arrow="false">
				<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
					<view class="uni-input">{{date}}</view>
				</picker>
			</u-cell-item>
			<u-cell-item title="手机号" :arrow="false">
				<input class="uni-input telNumber" maxlength="11" v-model="tel" placeholder="请输入手机号" />
			</u-cell-item>
			<u-cell-item title="住址" :arrow="false">
				<input class="uni-input address" v-model="address" placeholder="请输入住址" />
			</u-cell-item>
		</u-cell-group>
		<view class="below">
			<button class="clear" @click="clear" type="primary">取消</button>
			<button class="sub" @click="setPerson" type="primary">确定</button>
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
				nickName: '',
				sexCheck: '男',
				IdNum: '',
				sexlist: [{
						value: '男',
						name: '男',
						checked: 'true'
					},
					{
						value: '女',
						name: '女',
						checked: 'false'
					},
				],
				tel: '',
				address: '',
				current: 0,
				date: getDate({
					format: true
				}),
				startDate: getDate('start'),
				endDate: getDate('end'),
				ptCardId: null,
				mdType: 0
			}
		},
		onLoad(options) {
			this.mdType = options.type;
			if (options.type == 1) {
				uni.setNavigationBarTitle({
					title: '新增就诊人'
				});
			} else {
				uni.setNavigationBarTitle({
					title: '编辑就诊人信息'
				});
			}
			uni.$on('cardInfo', (res) => {
				console.log(res.data)
				this.nickName = res.data.name;
				this.current = res.data.sex - 1;
				this.IdNum = res.data.cardId;
				this.date = res.data.birthday;
				this.tel = res.data.tel;
				this.address = res.data.address;
				this.ptCardId = res.data.id;
			})
		},
		methods: {
			radioChange(evt) {
				this.sexCheck = evt.target.value;
				for (let i = 0; i < this.sexlist.length; i++) {
					if (this.sexlist[i].value === evt.target.value) {
						this.current = i;
						break;
					}
				}
			},
			setPerson() {
				if (this.nickName.length == 0 || this.IdNum.length == 0 || this.tel.length == 0 || this.address.length ==
					0) {
					uni.showToast({
						title: '请输入完整信息！',
						icon: 'none'
					})
				} else if (this.tel.length != 11) {
					uni.showToast({
						title: '请输入正确手机号！',
						icon: 'none'
					})
				} else if (this.IdNum.length != 18) {
					uni.showToast({
						title: '请输入真实身份证号！',
						icon: 'none'
					})
				} else {
					if (this.mdType == 2) { //编辑修改
						this.$u.put(api.RESET_PATIENT_INFO, {
							"id": this.ptCardId,
							"name": this.nickName,
							"cardId": this.IdNum,
							"tel": this.tel,
							"sex": this.current + 1,
							"birthday": this.date,
							"address": this.address
						}).then(res => {
							if (res.code == 200) {
								uni.showToast({
									title: '设置成功！',
									duration: 1000,
									success: function() {
										setTimeout(function() {
											uni.navigateTo({
												url: 'personList'
											})
										}, 500)
									}
								});

							} else {
								uni.showToast({
									title: '修改失败，请重试！',
									icon: 'none'
								});
							}

						})
					} else { //新增
						this.$u.post(api.RESET_PATIENT_INFO, {
							"name": this.nickName,
							"cardId": this.IdNum,
							"tel": this.tel,
							"sex": this.current + 1,
							"birthday": this.date,
							"address": this.address
						}).then(res => {
							if (res.code == 200) {
								uni.showToast({
									title: '添加成功！',
									duration: 2000,
									success: function() {
										setTimeout(function() {
											uni.navigateTo({
												url: 'personList'
											})
										}, 500)
									}
								});

							} else {
								uni.showToast({
									title: '修改失败，请重试！',
									icon: 'none'
								});
							}

						})
					}
				}
			},
			bindDateChange: function(e) {
				this.date = e.detail.value
			},
			clear() {
				uni.navigateTo({
					url: './personList',

				});
			}
		}
	}
</script>

<style scoped>
	.settingInfo {
		width: 100%;
		height: 100vh;
		background-color: #fff;
	}

	.nickName,
	.IdNumber,
	.telNumber,
	.address {
		text-align: right;
	}

	.radio {
		padding-left: 5px;
	}

	.confirmBtn {
		width: calc(100% - 40px);
		margin: 20px auto 0;
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
		margin-top: 20px;
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
