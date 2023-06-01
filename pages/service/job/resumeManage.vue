<template>
	<view class="manageWraper">
		<view class="jobSection">
			<view class="title">最高学历</view>
			<view class="itemInput">
				<input type="text" v-model="mostEducation" placeholder="最高学历" />
			</view>
		</view>
		<view class="jobSection">
			<view class="title">教育经历</view>
			<view class="itemInput">
				<textarea :auto-height="true" maxlength="84" v-model="education" placeholder="最多4行文字"></textarea>
			</view>
		</view>
		<view class="jobSection">
			<view class="title">居住地址</view>
			<view class="itemInput">
				<input type="text" v-model="address" placeholder="居住地址" />
			</view>
		</view>
		<view class="jobSection">
			<view class="title">工作经验</view>
			<view class="itemInput">
				<textarea :auto-height="true" v-model="experience" placeholder="最多4行文字"></textarea>
			</view>
		</view>
		<view class="jobSection">
			<view class="title">个人简介</view>
			<view class="itemInput">
				<textarea :auto-height="true" v-model="individualResume" placeholder="最多4行文字"></textarea>
			</view>
		</view>
		<view class="jobSection">
			<view class="title">期望薪资</view>
			<view class="itemInput">
				<input type="text" v-model="money" placeholder="期望薪资" />
			</view>
		</view>
		<view class="jobSection">
			<view class="title">个人简历</view>
			<view class="chooseFile">
				<input type="text" disabled="true" :value="fileName" placeholder="点击右侧按钮选择简历附件" />
				<u-icon @click.native="choosePdf" style="color: #007AFF;" type="upload-filled" size="30"></u-icon>
			</view>
		</view>
		<view class="jobSection">
			<view class="title">职位类别</view>
			<view class="form-item">
				<view class="form-label">选择职位</view>
				<picker @change="pickerChange" :value="postIndex.professionName" :range="postArr">
					<view class="uni-input">{{postArr[postIndex]}}</view>
				</picker>
			</view>
		</view>
		<view class="participate">
			<button type="primary" @click="addOrEdit">{{exist?'修改个人简历':'新增个人简历'}}</button>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js';
	export default {
		data() {
			return{
				mostEducation: '',
				education: '',
				address: '',
				experience: '',
				individualResume: '',
				money: '',
				positionId: '',
				files: '',
				fileName: '',
				postArr: [],
				postIndex: 0,
				original: [],
				exist: false,
				userId: 0
			}
		},
		onLoad(options) {
			this.userId = options.id;
			this.getPostlist();
		},
		methods:{
			getResumeDetails() {
				let userId = uni.getStorageSync('info').userId;
				this.$u.get(api.GET_RESUME_DETAIL+userId).then(res => {
					console.log(res)
					if(res.code == 200 && res.hasOwnProperty('data')) {
						this.exist = true;
						this.mostEducation = res.data.mostEducation;
						this.education = res.data.education;
						this.address = res.data.address;
						this.experience = res.data.experience;
						this.individualResume = res.data.individualResume;
						this.money = res.data.money;
						this.files = res.data.files;
						this.positionId = res.data.positionId;
						this.original.forEach((item,index) => {
							if(item.id == res.data.positionId){
								this.postIndex = index;
							}
						});
					}else {
						this.exist = false;
					}
				})
			},
			getPostlist() {
				this.$u.get(api.GET_PROFESSION_LIST).then(res => {
					if(res.code == 200){
						this.original = res.rows;
						this.original.forEach(item => {
							this.postArr.push(item.professionName)
						});
						this.getResumeDetails();
					}
				})
			},
			pickerChange: function(e) {
				this.postIndex = e.target.value;
				this.positionId = this.original[this.postIndex].id;
			},
			choosePdf() {
				let that = this;
				let token =  uni.getStorageSync('token');
				uni.chooseFile({
				  count: 1,
				  extension:['.pdf'],
				    success: function (res) {
				        // console.log(res.tempFilePaths[0]);
						that.fileName = res.tempFiles[0].name;
						uni.uploadFile({
							url: api.COMMON_UPLOAD, //仅为示例，非真实的接口地址
							filePath: res.tempFilePaths[0],
							name: 'file',
							header: {
								'authorization': token
							},
							success: (uploadFileRes) => {
								that.files = JSON.parse(uploadFileRes.data).fileName;
							}
						});
				    }
				});
			},
			addOrEdit() {
				if(this.exist) {//修改
					console.log(this.files)
					this.$u.put(api.ADD_EDIT_RESUME,{
						address: this.address,
						education: this.education,
						experience: this.experience,
						files: this.files,
						individualResume: this.individualResume,
						money: this.money,
						mostEducation: this.mostEducation,
						positionId: this.positionId
					}).then(res => {
						if(res.code == 200) {
							uni.showToast({
								icon: 'success',
								title: res.msg
							});
							this.getPostlist();
						} else {
							uni.showToast({
								icon: 'none',
								title: res.msg
							})
						}
					})
				} else {//新增
					this.$u.post(api.ADD_EDIT_RESUME,{
						address: this.address,
						education: this.education,
						experience: this.experience,
						files: this.files,
						individualResume: this.individualResume,
						money: this.money,
						mostEducation: this.mostEducation,
						positionId: this.positionId
					}).then(res => {
						console.log(res)
						if(res.code == 200) {
							uni.showToast({
								icon: 'success',
								title: res.msg
							});
							this.getPostlist();
						} else {
							uni.showToast({
								icon: 'none',
								title: res.msg
							})
						}
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.manageWraper{
		padding-bottom: 60px;
		.jobSection{
			// padding-top: 10px;
			.title{
				width: 100%;
				background-color: #d5d5d5;
				font-size: 16px;
				line-height: 26px;
				color: #333;
				text-indent: 1em;
			}
			.itemInput{
				box-sizing: border-box;
				padding: 8px 15px;
				input,textarea{
					width: 100%;
				}
			}
			.chooseFile{
				box-sizing: border-box;
				padding: 8px 15px;
				display: flex;
				align-items: center;
				input{
					flex: auto;
				}
			}
			.form-item{
				box-sizing: border-box;
				padding: 8px 15px;
				display: flex;
				align-items: center;
				justify-content: space-between;
				input{
					text-align: right;
				}
			}
		}
		.participate{
			width: calc(100% - 60px);
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
