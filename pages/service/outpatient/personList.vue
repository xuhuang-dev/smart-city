<template>
	<view>
		<view class="example-body">
			<view class="i-box u-flex" v-if="personList.length>0" v-for="(item,index) in personList" :key="index">
				<view class="cardLeft" @click="confirm(item.name)">
					<image src="/static/icons/defaultAt.png" class="personImg"></image>
				</view>
				<view class="u-flex-col cardTop" style="width: 100%;" @click="confirm(item.name)">
					<view class="cardName">{{item.name}}</view>
					<view class="cardText">身份证号：{{item.cardId}}</view>
					<view class="cardText">手 机 号 ：{{item.tel}}</view>
				</view>
				<view class="setPng" @click="uptPerson(item)">
					<image src="/static/icons/set.png" class="setImg"></image>
				</view>
			</view>
			<view class="i-box"isShadow @click="addPerson">
				<view class="u-flex u-row-center">
					<image src="/static/icons/add.png" class="setImg"></image>
					<text style="margin-left: 5px;">添加就诊人</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import api from '../../../common/apiUtil.js'
	export default {
		data() {
			return {
				personList: []
			}
		},
		created() {
			this.getCardlist();
		},
		methods: {
			getCardlist() {
				let token = uni.getStorageSync("token")
				if(token==null||token.length==0){
					this.$u.toast("请先登录")
					setTimeout(()=>{
						this.$u.route({
							url:"/pages/login/login",
							type:"to"
						})
					},1000)
				}
				let user = uni.getStorageSync('info');
				console.log("getCardlist.user",user)
				this.$u.get(api.GET_PATIENT_LIST, {
					userId: user.userId
				}).then(res => {
					this.personList = res.rows;
					console.log("personListRes",res)
				})
			},
			addPerson() {
				uni.navigateTo({
					url: './addperson?type=1'
				});
			},
			uptPerson(item) {
				setTimeout(function() {
					uni.$emit('cardInfo', {
						data: item
					});
				}, 500);

				uni.navigateTo({
					url: './addperson?type=2'
				});
			},
			confirm(name) {
				uni.navigateTo({
					url: './registration?name=' + name
				});
			},
		}
	}
</script>

<style>
	.uni-card {
		height: 100px;
	}

	.personImg {
		width: 50px;
		height: 50px;
		border-radius: 50%;
	}

	.cardLeft {
		/* float: left; */
		height: 100%;
		/* margin-top: 2%; */
	}

	.cardTop {
		width: 100%;
		padding: 4px 10px;
	}

	.cardName {
		float: left;
	}

	.setPng {
		float: right;
	}

	.setImg {
		width: 25px;
		height: 25px;
		vertical-align: middle;
	}

	.cardBottom {
		position: absolute;
		right: 25px;
		bottom: 15px;
		width: 65%;
		height: 30%;
		border: 1px solid #ECECEC;
		padding: 4px;
	}

	.cardText {
		font-size: 13px;
		color: #666666;
	}

	.addPerson {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}
</style>
