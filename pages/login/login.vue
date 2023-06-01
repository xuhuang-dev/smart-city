<template>
	<view class="wrap">
		<view class="top"></view>
		<view class="content">
			<view class="title">欢迎登录</view>
			<input class="u-border-bottom" type="text" v-model="username" placeholder="请输入用户名" />
			<input class="u-border-bottom" type="password" v-model="password" placeholder="请输入密码" />
			<button @tap="submit" :style="[inputStyle]" class="getCaptcha">登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 124.93.196.45:10001
				// username: "xuhuang",
				// password: "123123",
				username: "",
				password: "",
				baseUrl: "",
				tel: false
			}
		},
		onLoad() {
			this.baseUrl = this.$u.http.config.baseUrl
		},
		watch: {
			username(newValue) {
				if (newValue.length > 0 && this.password.length > 0) this.tel = true
				else this.tel = false
			},
			password(newValue) {
				if (newValue.length > 0 && this.username.length > 0) this.tel = true
				else this.tel = false
			},
		},
		computed: {
			inputStyle() {
				let style = {};
				if (this.tel) {
					style.color = "#fff";
					style.backgroundColor = '#ffc65d';
					// style.backgroundColor = this.$u.color['warning'];
				}
				return style;
			}
		},
		methods: {
			getUserInfo() {
				this.$u.get("/prod-api/api/common/user/getInfo").then(res => {
					console.log(1)
					uni.setStorageSync("info",res.user)
					console.log(2)
				})
			},
			submit() {
				if (this.username.length == 0) {
					this.$u.toast("请输入用户名")
					return
				}
				if (this.password.length == 0) {
					this.$u.toast("请输入密码")
					return
				}
				this.$u.post("/prod-api/api/login", {
					"username": this.username,
					"password": this.password
				}).then(res => {
					uni.setStorageSync("token", res.token)
					this.getUserInfo()
					console.log(3)
					this.$u.route({
						url: "/pages/home/home",
						type: 'tab'
					})
				})
			}
		}
	};
</script>
</style>
<style lang="scss" scoped>
	.wrap {
		display: flex;
		justify-content: center;
		font-size: 28rpx;

		.content {
			width: 600rpx;
			margin: 80rpx;

			.title {
				text-align: left;
				font-size: 60rpx;
				font-weight: 500;
				margin-bottom: 100rpx;
			}

			input {
				text-align: left;
				margin-bottom: 10rpx;
				padding-bottom: 6rpx;
			}

			.tips {
				color: $u-type-info;
				margin-bottom: 60rpx;
				margin-top: 8rpx;
			}

			.getCaptcha {
				background-color: rgb(253, 243, 208);
				color: $u-tips-color;
				border: none;
				font-size: 30rpx;
				padding: 12rpx 0;

				&::after {
					border: none;
				}
			}

			.alternative {
				color: $u-tips-color;
				display: flex;
				justify-content: space-between;
				margin-top: 30rpx;
			}
		}

		.buttom {
			.loginType {
				display: flex;
				padding: 350rpx 150rpx 150rpx 150rpx;
				justify-content: space-between;

				.item {
					display: flex;
					flex-direction: column;
					align-items: center;
					color: $u-content-color;
					font-size: 28rpx;
				}
			}

			.hint {
				padding: 20rpx 40rpx;
				font-size: 20rpx;
				color: $u-tips-color;

				.link {
					color: $u-type-warning;
				}
			}
		}
	}
</style>
