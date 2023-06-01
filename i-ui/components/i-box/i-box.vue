<template>
	<view class="i-box i-border" :class="{ 'i-box--full': isFull === true || isFull === 'true', 'i-box--shadow': isShadow === true || isShadow === 'true'}">
		<!-- 基础 -->
		<view v-if="mode === 'basic' && title" class="i-box__header i-border-bottom" @click.stop="onClick">
			<view v-if="thumbnail" class="i-box__header-extra-img-view">
				<image :src="thumbnail" class="i-box__header-extra-img" />
			</view>
			<text class="i-box__header-title-text">{{ title }}</text>
			<text v-if="extra" class="i-box__header-extra-text">{{ extra }}</text>
		</view>
		<!-- 标题 -->
		<view v-if="mode === 'title'" class="i-box__title i-border-bottom" @click.stop="onClick">
			<view class="i-box__title-box">
				<view class="i-box__title-header">
					<image class="i-box__title-header-image" :src="thumbnail" mode="scaleToFill" />
				</view>
				<view class="i-box__title-content">
					<text class="i-box__title-content-title i-ellipsis">{{ title }}</text>
					<text class="i-box__title-content-extra i-ellipsis">{{ subTitle }}</text>
				</view>
			</view>
			<view v-if="extra">
				<text class="i-box__header-extra-text">{{ extra }}</text>
			</view>
		</view>
		<!-- 图文 -->
		<view v-if="mode === 'style'" class="i-box__thumbnailimage" @click.stop="onClick">
			<view class="i-box__thumbnailimage-box">
				<image class="i-box__thumbnailimage-image" :src="thumbnail" mode="aspectFill" />
			</view>
			<view v-if="title" class="i-box__thumbnailimage-title"><text class="i-box__thumbnailimage-title-text">{{ title }}</text></view>
		</view>
		<!-- 内容 -->
		<view class="i-box__content i-box__content--pd" @click.stop="onClick">
			<view v-if="mode === 'style' && extra" class=""><text class="i-box__content-extra">{{ extra }}</text></view>
			<slot />
		</view>
		<!-- 底部 -->
		<view v-if="note" class="i-box__footer i-border-top">
			<slot name="footer">
				<text class="i-box__footer-text">{{ note }}</text>
			</slot>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'IBox',
		data() {
			return {
				
			}
		},
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
.i-box{
	background-color: white;
	border-radius: 20rpx;
	margin: 0 20rpx 0 20rpx;
	border: 1 solid #f3f4f6;
	overflow: hidden;
}
</style>
<style scoped>
	.i-box {
		/* #ifndef APP-NVUE */
		display: flex;
		flex: 1;
		box-shadow: 0 0 0 rgba(0, 0, 0, 0);
		/* #endif */
		flex-direction: column;
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

	.i-box__thumbnailimage {
		position: relative;
		flex-direction: column;
		justify-content: center;
		height: 150px;
		overflow: hidden;
	}

	.i-box__thumbnailimage-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		overflow: hidden;
	}

	.i-box__thumbnailimage-image {
		flex: 1;
	}

	.i-box__thumbnailimage-title {
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

	.i-box__thumbnailimage-title-text {
		flex: 1;
		font-size: 14px;
		color: #fff;
	}

	.i-box__title {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
		padding: 10px;

	}

	.i-box__title-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		align-items: center;
		overflow: hidden;
	}

	.i-box__title-header {
		width: 40px;
		height: 40px;
		overflow: hidden;
		border-radius: 5px;
	}

	.i-box__title-header-image {
		width: 40px;
		height: 40px;
	}

	.i-box__title-content {
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

	.i-box__title-content-title {
		font-size: 14px;
		line-height: 22px;
	}

	.i-box__title-content-extra {
		font-size: 12px;
		line-height: 27px;
		color: #999;
	}

	.i-box__header {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		position: relative;
		flex-direction: row;
		padding: 12px;
		align-items: center;
	}

	.i-box__header-title {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		margin-right: 8px;
		justify-content: flex-start;
		align-items: center;
	}

	.i-box__header-title-text {
		font-size: 16;
		flex: 1;
		color: #333;
	}

	.i-box__header-extra-img {
		height: 20px;
		width: 20px;
		margin-right: 8px;
	}

	.i-box__header-extra-text {
		flex: 1;
		margin-left: 8px;
		font-size: 12px;
		text-align: right;
		color: #999;
	}

	.i-box__content {
		color: #333;
	}

	.i-box__content--pd {
		padding: 12px;
	}

	.i-box__content-extra {
		font-size: 14px;
		padding-bottom: 10px;
		color: #999;
	}

	.i-box__footer {
		justify-content: space-between;
		padding: 12px;
	}

	.i-box__footer-text {
		color: #999;
		font-size: 12px;
	}

	.i-box--shadow {
		position: relative;
		/* #ifndef APP-NVUE */
		box-shadow: 0px 0px 5px 1px rgba(0, 0, 0, 0.1);
		/* #endif */
	}

	.i-box--full {
		margin: 0;
		border-radius: 0;
	}

	/* #ifndef APP-NVUE */
	.i-box--full:after {
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