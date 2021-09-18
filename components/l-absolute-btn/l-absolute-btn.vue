<template>
	<view class="absolute-btn" :style="{
		backgroundColor: bgColor,
		right: right,
		bottom: bottom
	}" @click="clickEvent">
		<image :src="src"></image>
	</view>
</template>

<script>
	/**
	 * v-absolute-btn 悬浮在右下角的圆形按钮
	 * @description 悬浮按钮用于某些操作，事件可自定义
	 * @property {String} imgSrc 传入的图标路径，需为png格式，大小128*128（默认为 “扫码图标”）
	 * @property {String} bgColor 按钮背景颜色（默认为系统主体色）
	 */
	export default {
		name: "v-absolute-btn",
		props: {
			imgSrc: {
				default: 'change',
				type: String
			},
			bgColor: {
				default: '#6C9EFF',
				type: String
			},
			// 右边距
			right: {
				default: '25px',
				type: String
			},
			// 底边距
			bottom: {
				default: '50px',
				type: String
			}
		},
		computed: {
			src() {
				return require('../../static/' + this.imgSrc + '.png')
			},
			btnStyle() {
				return {
					'backgroundColor': this.bgColor
				}
			}
		},
		data() {
			return {

			};
		},
		methods: {
			clickEvent() {
				this.$emit('click')
			}
		}
	}
</script>

<style lang="scss">
	.absolute-btn {
		width: 120rpx;
		height: 120rpx;
		position: fixed;
		// right: 50rpx;
		// bottom: 100rpx;
		opacity: 0.6;
		border-radius: 50%;
		display: flex;
		justify-content: center;

		>image {
			width: 60rpx;
			height: 60rpx;
			align-self: center;
		}

		&:active {
			opacity: 1;
			transition: 0.2s;
		}
	}
</style>
