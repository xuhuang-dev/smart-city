<template>
	<view class="u-p-20">
			<view class="" v-if="swiperCurrent==0">
				<view class="u-flex">
					<view class="u-m-r-10" @click="isSelectLocal=!isSelectLocal">
						<u-icon name="map" size="38" color="#ccc"></u-icon>
						<text>{{local}}</text>
					</view>
					<u-search v-model="searchText" :showAction="false" :clearabled="false" @custom="search" @search="search"></u-search>
				</view>
				<u-index-list :scrollTop="scrollTop" :index-list="indexList" v-show="isSelectLocal" class="u-m-t-20" :sticky="false">
					<view v-for="(item, index) in list" :key="index">
						<u-index-anchor :index="item.letter" />
						<view class="list-cell u-border-bottom" v-for="(item1, index) in item.data" :key="index" @click="selectLocation(item1)">
							{{item1.name}}
						</view>
					</view>
				</u-index-list>
				<view class="title-lg">附近商家</view>
				<!-- 显示附近商家模块，多行显示，每行显示一个店家信息，店家信息布局为左侧店家封面，右侧上方显示店家名称1、评分1（最多 5 分）、月销量1、到店所需时间1、到店距离1、人均消费1，点击店家信息进入店家详情页 -->
				<view class="page-box">
					<view class="order" v-for="(item, index) in newsList" :key="item.id" @click="$u.route({url:'/pages/service/takeout/sale-detail',params:{id:item.id}})">
						<view class="item" >
							<view class="left"><image :src="baseUrl+item.imgUrl" mode="aspectFill"></image></view>
							<view class="content u-flex-1 u-flex-col u-row-around">
								<view class="u-flex u-row-between">
									<view class="title u-line-1">{{ item.name }}</view>
									<view class="u-font-20">{{ item.distance }}m</view>
								</view>
								
								<view class="delivery-time u-flex u-row-between">
									<view class="">月销量：{{ item.saleQuantity }}</view>
									<view class="">人均消费：{{ item.avgCost }}￥</view>
								</view>
								<view class="delivery-time u-flex u-row-between">
									<view class="">评分：{{ item.score }}</view>
									<view class="">到店所需时间：{{ item.deliveryTime }}min</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="" v-else-if="swiperCurrent==1">123</view>
		<view class="u-tabs-box">
			<u-tabbar
					v-model="swiperCurrent"
					:list="list"
					activeColor="#000"
			></u-tabbar>
		</view>
	</view>
</template>

<script>
	import indexList from "@/common/index.list.js";
	const letterArr = indexList.list.map(val => {
		return val.letter;
	})
	export default {
		data() {
			return {
				baseUrl:"",
				local:"杭州",
				searchText:"",
				scrollTop: 0,
				indexList: letterArr,
				list: indexList.list,
				isSelectLocal:false,
				orderList: [[], [], [], []],
				dataList: [
					{
						id: 1,
						store: '夏日流星限定贩卖',
						deal: '交易成功',
						goodsList: [
							{
								goodsUrl: '//img13.360buyimg.com/n7/jfs/t1/103005/7/17719/314825/5e8c19faEb7eed50d/5b81ae4b2f7f3bb7.jpg',
								title: '【冬日限定】现货 原创jk制服女2020冬装新款小清新宽松软糯毛衣外套女开衫短款百搭日系甜美风',
								type: '灰色;M',
								deliveryTime: '付款后30天内发货',
								price: '348.58',
								number: 2
							},
							{
								goodsUrl: '//img12.360buyimg.com/n7/jfs/t1/102191/19/9072/330688/5e0af7cfE17698872/c91c00d713bf729a.jpg',
								title: '【葡萄藤】现货 小清新学院风制服格裙百褶裙女短款百搭日系甜美风原创jk制服女2020新款',
								type: '45cm;S',
								deliveryTime: '付款后30天内发货',
								price: '135.00',
								number: 1
							}
						]
					},
					{
						id: 2,
						store: '江南皮革厂',
						deal: '交易失败',
						goodsList: [
							{
								goodsUrl: '//img14.360buyimg.com/n7/jfs/t1/60319/15/6105/406802/5d43f68aE9f00db8c/0affb7ac46c345e2.jpg',
								title: '【冬日限定】现货 原创jk制服女2020冬装新款小清新宽松软糯毛衣外套女开衫短款百搭日系甜美风',
								type: '粉色;M',
								deliveryTime: '付款后7天内发货',
								price: '128.05',
								number: 1
							}
						]
					},
					{
						id: 3,
						store: '三星旗舰店',
						deal: '交易失败',
						goodsList: [
							{
								goodsUrl: '//img11.360buyimg.com/n7/jfs/t1/94448/29/2734/524808/5dd4cc16E990dfb6b/59c256f85a8c3757.jpg',
								title: '三星（SAMSUNG）京品家电 UA65RUF70AJXXZ 65英寸4K超高清 HDR 京东微联 智能语音 教育资源液晶电视机',
								type: '4K，广色域',
								deliveryTime: '保质5年',
								price: '1998',
								number: 3
							},
							{
								goodsUrl: '//img14.360buyimg.com/n7/jfs/t6007/205/4099529191/294869/ae4e6d4f/595dcf19Ndce3227d.jpg!q90.jpg',
								title: '美的(Midea)639升 对开门冰箱 19分钟急速净味 一级能效冷藏双开门杀菌智能家用双变频节能 BCD-639WKPZM(E)',
								type: '容量大，速冻',
								deliveryTime: '保质5年',
								price: '2354',
								number: 1
							}
						]
					},
					{
						id: 4,
						store: '三星旗舰店',
						deal: '交易失败',
						goodsList: [
							{
								goodsUrl: '//img10.360buyimg.com/n7/jfs/t22300/31/1505958241/171936/9e201a89/5b2b12ffNe6dbb594.jpg!q90.jpg',
								title: '法国进口红酒 拉菲（LAFITE）传奇波尔多干红葡萄酒750ml*6整箱装',
								type: '4K，广色域',
								deliveryTime: '珍藏10年好酒',
								price: '1543',
								number: 3
							},
							{
								goodsUrl: '//img10.360buyimg.com/n7/jfs/t1/107598/17/3766/525060/5e143aacE9a94d43c/03573ae60b8bf0ee.jpg',
								title: '蓝妹（BLUE GIRL）酷爽啤酒 清啤 原装进口啤酒 罐装 500ml*9听 整箱装',
								type: '一打',
								deliveryTime: '口感好',
								price: '120',
								number: 1
							}
						]
					},
					{
						id: 5,
						store: '三星旗舰店',
						deal: '交易成功',
						goodsList: [
							{
								goodsUrl: '//img12.360buyimg.com/n7/jfs/t1/52408/35/3554/78293/5d12e9cfEfd118ba1/ba5995e62cbd747f.jpg!q90.jpg',
								title: '企业微信 中控人脸指纹识别考勤机刷脸机 无线签到异地多店打卡机WX108',
								type: '识别效率高',
								deliveryTime: '使用方便',
								price: '451',
								number: 9
							}
						]
					}
				],
				list: [{
						iconPath: "home",
						selectedIconPath: "home",
						text: '首页',
						customIcon: false,
					},
					{
						iconPath: "order",
						selectedIconPath: "order",
						
						text: '订单',
						customIcon: false,
					},
					{
						iconPath: "account",
						selectedIconPath: "account",
						text: '我的',
						count: 23,
						isDot: false,
						customIcon: false,
					},
				],
				swiperCurrent: 0,
				tabsHeight: 0,
				dx: 0,
				newsList:[],
			}
		},
		onLoad() {
			this.baseUrl = this.$u.http.config.baseUrl
			this.getNews();
		},
		onPageScroll(e) {
			this.scrollTop = e.scrollTop;
		},
		methods:{
			getNews(){
				this.$u.get("/prod-api/api/takeout/seller/near").then(res=>{
					this.newsList = res.rows
					console.log("newsList",this.newsList)
				})
			},
			search(e){
				if(e.length==0){
					this.$u.toast("请输入内容")
					return
				}
				this.$u.get("/prod-api/press/press/list",{title:e}).then(res=>{
					if(res.total==0)					this.$u.toast("未找到")
					else{
						this.$u.route({url:"/pages/home/result",params:{title:e}})
					}
				})
			},
			selectLocation(e){
				this.local = e.name
				// console.log(e)
				this.isSelectLocal = false
			},
			reachBottom() {
				// 此tab为空数据
				if(this.current != 2) {
					this.loadStatus.splice(this.current,1,"loading")
					setTimeout(() => {
						this.getOrderList(this.current);
					}, 1200);
				}
			},
			// 页面数据
			getOrderList(idx) {
				for(let i = 0; i < 5; i++) {
					let index = this.$u.random(0, this.dataList.length - 1);
					let data = JSON.parse(JSON.stringify(this.dataList[index]));
					data.id = this.$u.guid();
					this.orderList[idx].push(data);
				}
				this.loadStatus.splice(this.current,1,"loadmore")
			},
			// 总价
			totalPrice(item) {
				let price = 0;
				item.map(val => {
					price += parseFloat(val.price);
				});
				return price.toFixed(2);
			},
			// 总件数
			totalNum(item) {
				let num = 0;
				item.map(val => {
					num += val.number;
				});
				return num;
			},
			// tab栏切换
			change(index) {
				this.swiperCurrent = index;
				this.getOrderList(index);
			},
			transition({ detail: { dx } }) {
				this.$refs.tabs.setDx(dx);
			},
			animationfinish({ detail: { current } }) {
				this.$refs.tabs.setFinishCurrent(current);
				this.swiperCurrent = current;
				this.current = current;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.u-demo-area {
		margin: 0 -40rpx;
	}
	.list-cell {
		display: flex;
		box-sizing: border-box;
		width: 100%;
		padding: 10px 24rpx;
		overflow: hidden;
		color: $u-content-color;
		font-size: 14px;
		line-height: 24px;
		background-color: #fff;
	}

.order {
	width: 710rpx;
	background-color: #ffffff;
	margin: 20rpx auto;
	border-radius: 20rpx;
	box-sizing: border-box;
	padding: 20rpx;
	font-size: 28rpx;
	.top {
		display: flex;
		justify-content: space-between;
		.left {
			display: flex;
			align-items: center;
			.store {
				margin: 0 10rpx;
				font-size: 32rpx;
				font-weight: bold;
			}
		}
		.right {
			color: $u-type-warning-dark;
		}
	}
	.item {
		display: flex;
		margin: 20rpx 0 0;
		.left {
			margin-right: 20rpx;
			image {
				width: 200rpx;
				height: 200rpx;
				border-radius: 10rpx;
			}
		}
		.content {
			.title {
				font-size: 28rpx;
				line-height: 50rpx;
			}
			.type {
				margin: 10rpx 0;
				font-size: 24rpx;
				color: $u-tips-color;
			}
			.delivery-time {
				color: #e5d001;
				font-size: 24rpx;
			}
		}
		.right {
			margin-left: 10rpx;
			padding-top: 20rpx;
			text-align: right;
			.decimal {
				font-size: 24rpx;
				margin-top: 4rpx;
			}
			.number {
				color: $u-tips-color;
				font-size: 24rpx;
			}
		}
	}
	.total {
		margin-top: 20rpx;
		text-align: right;
		font-size: 24rpx;
		.total-price {
			font-size: 32rpx;
		}
	}
	.bottom {
		display: flex;
		margin-top: 40rpx;
		padding: 0 10rpx;
		justify-content: space-between;
		align-items: center;
		.btn {
			line-height: 52rpx;
			width: 160rpx;
			border-radius: 26rpx;
			border: 2rpx solid $u-border-color;
			font-size: 26rpx;
			text-align: center;
			color: $u-type-info-dark;
		}
		.evaluate {
			color: $u-type-warning-dark;
			border-color: $u-type-warning-dark;
		}
	}
}
.centre {
	text-align: center;
	margin: 200rpx auto;
	font-size: 32rpx;
	image {
		width: 164rpx;
		height: 164rpx;
		border-radius: 50%;
		margin-bottom: 20rpx;
	}
	.tips {
		font-size: 24rpx;
		color: #999999;
		margin-top: 20rpx;
	}
	.btn {
		margin: 80rpx auto;
		width: 200rpx;
		border-radius: 32rpx;
		line-height: 64rpx;
		color: #ffffff;
		font-size: 26rpx;
		background: linear-gradient(270deg, rgba(249, 116, 90, 1) 0%, rgba(255, 158, 1, 1) 100%);
	}
}
.wrap {
	display: flex;
	flex-direction: column;
	height: calc(100vh - var(--window-top));
	width: 100%;
}
.swiper-box {
	flex: 1;
}
.swiper-item {
	height: 100%;
}
</style>
