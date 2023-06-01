<template>
	<view class="i-card i-border" :class="{ 'i-card--full': isFull === true || isFull === 'true', 'i-card--shadow': isShadow === true || isShadow === 'true'}">
		<!-- 基础 -->
		<view v-if="mode === 'basic' && title" class="i-card__header i-border-bottom" @click.stop="onClick">
			<view v-if="thumbnail" class="i-card__header-extra-img-view">
				<image :src="thumbnail" class="i-card__header-extra-img" />
			</view>
			<text class="i-card__header-title-text">{{ title }}</text>
			<text v-if="extra" class="i-card__header-extra-text">{{ extra }}</text>
		</view>
		<!-- 标题 -->
		<view v-if="mode === 'title'" class="i-card__title i-border-bottom" @click.stop="onClick">
			<view class="i-card__title-box">
				<view class="i-card__title-header">
					<image class="i-card__title-header-image" :src="thumbnail" mode="scaleToFill" />
				</view>
				<view class="i-card__title-content">
					<text class="i-card__title-content-title i-ellipsis">{{ title }}</text>
					<text class="i-card__title-content-extra i-ellipsis">{{ subTitle }}</text>
				</view>
			</view>
			<view v-if="extra">
				<text class="i-card__header-extra-text">{{ extra }}</text>
			</view>
		</view>
		<!-- 图文 -->
		<view v-if="mode === 'style'" class="i-card__thumbnailimage" @click.stop="onClick">
			<view class="i-card__thumbnailimage-box">
				<image class="i-card__thumbnailimage-image" :src="thumbnail" mode="aspectFill" />
			</view>
			<view v-if="title" class="i-card__thumbnailimage-title"><text class="i-card__thumbnailimage-title-text">{{ title }}</text></view>
		</view>
		<!-- 内容 -->
		<view class="i-card__content i-card__content--pd" @click.stop="onClick">
			<view v-if="mode === 'style' && extra" class=""><text class="i-card__content-extra">{{ extra }}</text></view>
			<slot />
		</view>
		<!-- 底部 -->
		<view v-if="note" class="i-card__footer i-border-top">
			<slot name="footer">
				<text class="i-card__footer-text">{{ note }}</text>
			</slot>
		</view>
	</view>
</template>

<script>
	/**
	 * Card 卡片
	 * @description 卡片视图组件
	 * @tutorial https://ext.dcloud.net.cn/plugin?id=22
	 * @property {String} title 标题文字
	 * @property {String} subTitle 副标题（仅仅mode=title下生效）
	 * @property {String} extra 标题额外信息
	 * @property {String} note 标题左侧缩略图
	 * @property {String} thumbnail 底部信息
	 * @property {String} mode = [basic|style|title] 卡片模式
	 * 	@value basic 基础卡片
	 * 	@value style 图文卡片
	 * 	@value title 标题卡片
	 * @property {Boolean} isFull = [true | false] 卡片内容是否通栏，为 true 时将去除padding值
	 * @property {Boolean} isShadow = [true | false] 卡片内容是否开启阴影
	 * @event {Function} click 点击 Card 触发事件
	 * @example <i-card title="标题文字" thumbnail="https://img-cdn-qiniu.dcloud.net.cn/new-page/i.png" extra="额外信息" note="Tips">内容主体，可自定义内容及样式</i-card>
	 */
	export default {
		name: 'ICard',
		props: {
			title: {
				type: String,
				default: ''
			},
			subTitle: {
				type: String,
				default: ''
			},
			extra: {
				type: String,
				default: ''
			},
			note: {
				type: String,
				default: ''
			},
			thumbnail: {
				type: String,
				default: ''
			},
			mode: {
				type: String,
				default: 'basic'
			},
			isFull: {
				// 内容区域是否通栏
				type: Boolean,
				default: false
			},
			isShadow: {
				// 是否开启阴影
				type: [Boolean, String],
				default: false
			}
		},
		methods: {
			onClick() {
				this.$emit('click')
			}
		}
	}
</script>

<style scoped>
	.i-card {
		/* #ifndef APP-NVUE */
		display: flex;
		flex: 1;
		box-shadow: 0 0 0 rgba(0, 0, 0, 0);
		/* #endif */
		margin: 12px 15px;
		background-color: #ffffff;
		position: relative;
		flex-direction: column;
		border-radius: 5px;
		overflow: hidden;
	}



	.i-border {
		position: relative;
		/* #ifdef APP-NVUE */
		border-color: #e5e5e5;
		border-style: solid;
		border-width: 0.5px;
		/* #endif */
		z-index: 1;
	}

	/* #ifndef APP-NVUE */
	.i-border:after {
		content: '';
		position: absolute;
		bottom: 0;
		left: 0;
		top: 0;
		right: 0;
		border: 1px solid #e5e5e5;
		border-radius: 10px;
		box-sizing: border-box;
		width: 200%;
		height: 200%;
		transform: scale(0.5);
		transform-origin: left top;
		z-index: -1;
	}

	/* #endif */

	.i-border-bottom {
		position: relative;
		/* #ifdef APP-NVUE */
		border-bottom-color: #e5e5e5;
		border-bottom-style: solid;
		border-bottom-width: 0.5px;
		/* #endif */
		z-index: 1;
	}

	/* #ifndef APP-NVUE */
	.i-border-bottom:after {
		content: '';
		position: absolute;
		bottom: 0;
		left: 0;
		top: 0;
		right: 0;
		border-bottom: 1px solid #e5e5e5;
		box-sizing: border-box;
		width: 200%;
		height: 200%;
		transform: scale(0.5);
		transform-origin: left top;
		z-index: -1;
	}

	/* #endif */
	.i-border-top {
		position: relative;
		/* #ifdef APP-NVUE */
		border-top-color: #e5e5e5;
		border-top-style: solid;
		border-top-width: 0.5px;
		/* #endif */
		z-index: 1;
	}

	/* #ifndef APP-NVUE */
	.i-border-top:after {
		content: '';
		position: absolute;
		bottom: 0;
		left: 0;
		top: 0;
		right: 0;
		border-top: 1px solid #e5e5e5;
		box-sizing: border-box;
		width: 200%;
		height: 200%;
		transform: scale(0.5);
		transform-origin: left top;
		z-index: -1;
	}

	/* #endif */

	.i-card__thumbnailimage {
		position: relative;
		flex-direction: column;
		justify-content: center;
		height: 150px;
		overflow: hidden;
	}

	.i-card__thumbnailimage-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		overflow: hidden;
	}

	.i-card__thumbnailimage-image {
		flex: 1;
	}

	.i-card__thumbnailimage-title {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		flex-direction: row;
		padding: 8px 12px;
		background-color: rgba(0, 0, 0, 0.4);
	}

	.i-card__thumbnailimage-title-text {
		flex: 1;
		font-size: 14px;
		color: #fff;
	}

	.i-card__title {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		padding: 10px;

	}

	.i-card__title-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		align-items: center;
		overflow: hidden;
	}

	.i-card__title-header {
		width: 40px;
		height: 40px;
		overflow: hidden;
		border-radius: 5px;
	}

	.i-card__title-header-image {
		width: 40px;
		height: 40px;
	}

	.i-card__title-content {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		justify-content: center;
		flex: 1;
		padding-left: 10px;
		height: 40px;
		overflow: hidden;
	}

	.i-card__title-content-title {
		font-size: 14px;
		line-height: 22px;
	}

	.i-card__title-content-extra {
		font-size: 12px;
		line-height: 27px;
		color: #999;
	}

	.i-card__header {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		position: relative;
		flex-direction: row;
		padding: 12px;
		align-items: center;
	}

	.i-card__header-title {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		margin-right: 8px;
		justify-content: flex-start;
		align-items: center;
	}

	.i-card__header-title-text {
		font-size: 16;
		flex: 1;
		color: #333;
	}

	.i-card__header-extra-img {
		height: 20px;
		width: 20px;
		margin-right: 8px;
	}

	.i-card__header-extra-text {
		flex: 1;
		margin-left: 8px;
		font-size: 12px;
		text-align: right;
		color: #999;
	}

	.i-card__content {
		color: #333;
	}

	.i-card__content--pd {
		padding: 12px;
	}

	.i-card__content-extra {
		font-size: 14px;
		padding-bottom: 10px;
		color: #999;
	}

	.i-card__footer {
		justify-content: space-between;
		padding: 12px;
	}

	.i-card__footer-text {
		color: #999;
		font-size: 12px;
	}

	.i-card--shadow {
		position: relative;
		/* #ifndef APP-NVUE */
		box-shadow: 0px 0px 5px 1px rgba(0, 0, 0, 0.1);
		/* #endif */
	}

	.i-card--full {
		margin: 0;
		border-radius: 0;
	}

	/* #ifndef APP-NVUE */
	.i-card--full:after {
		border-radius: 0;
	}

	/* #endif */
	.i-ellipsis {
		/* #ifndef APP-NVUE */
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		/* #endif */
		/* #ifdef APP-NVUE */
		lines: 1
			/* #endif */
	}
</style>