<template>
	<view class="container">
		<!-- 题目名称 -->
		<view class="title">
			{{`(${count}/${maxCount})`+' '}}{{showObj.question}}
		</view>
		<!-- 按钮 -->
		<l-button @click="seeAnswer" type="warn" width="100%">查看答案</l-button>
		<!-- 答案区域-图 -->
		<view v-show="showObj.imgSrc && isShowAnswer" class="answer">
			<!-- aspectFit: 不缩放，保证长边可完全显示 -->
			<image mode="aspectFit" :src="showObj.imgSrc" @click="previewImage(showObj.imgSrc)"></image>
		</view>
		<!-- 答案区域-文字 -->
		<view class="answer-text" v-show="isShowAnswer">
			<ul>
				<li v-for="(item,index) in showObj.answerText" class="li">{{item}}</li>
			</ul>
		</view>
		<!-- 底部安全区域 -->
		<view class="safeArea">
		</view>
		<l-absolute-btn bgColor="#96bf96" @click="nextQuestion" bottom="100px"></l-absolute-btn>
	</view>
</template>

<script>
	import questionList from '../../common/questionList.js'
	export default {
		computed: {
			maxCount() {
				return this.list.length
			}
		},
		data() {
			return {
				isShowAnswer: false,
				list: JSON.parse(JSON.stringify(questionList)),
				showObj: null,
				// 计数器，最大值为当前数组的长度
				count: 0
			}
		},
		onLoad() {
			this.showObj = this.getRandomItem(this.list)
		},
		methods: {
			// 重置当前页面
			initPage() {
				this.count = 0
				this.list = JSON.parse(JSON.stringify(questionList))
				this.isShowAnswer = false
				this.$nextTick(() => {
					this.showObj = this.getRandomItem(this.list)
				})
			},
			// 预览图片
			previewImage(src) {
				uni.previewImage({
					urls: [src]
				})
			},
			// 查看答案
			seeAnswer() {
				this.isShowAnswer = !this.isShowAnswer
			},
			// 下一题
			nextQuestion() {
				// 当随机完所有题目时，则无法重新随机
				if (this.count >= this.maxCount) {
					// uni.showToast({
					// 	title: '无更多题目！'
					// });
					uni.showModal({
						title: '无更多题目',
						content: '是否重来一次？',
						success: (res) => {
							if (res.confirm) {
								// 重来
								this.initPage()
							} else if (res.cancel) {
								// 不重来
							}
						}
					});
					return
				}
				this.showObj = this.getRandomItem(this.list)
				this.isShowAnswer = false
			},
			/**
			 * @description 获取数组的随机一项
			 * @param {Array} arr 用于随机的数组
			 * @return {any} 数组的任一项
			 */
			getRandomItem(arr) {
				if (!Array.isArray(arr)) {
					throw Error('Invalid params, please enter an Array type')
					return
				}

				const array = arr.slice()

				// floor: 向下取整
				let min = 0;
				let max = Math.floor(arr.length);

				// random 取值 [0 ~ 1] 的浮点数
				// 随机值 [min, max]，包含 min，不包含 max（因为 length 比 index 大 1，所以直接用 max 作为数组索引即可）
				let randomIndex = Math.floor(Math.random() * (max - min)) + min;

				// 标记已阅，若当前为已阅则重新取值
				while (this.list[randomIndex].isRead) {
					randomIndex = Math.floor(Math.random() * (max - min)) + min;
				}

				// 已阅的题目做标记
				this.list[randomIndex].isRead = true
				// 每次成功随机一道题，则计数器+1
				this.count++

				return array[randomIndex]
			}
		}
	}
</script>

<style lang="scss">
	$marginTop: 40rpx;

	.container {
		padding: 0 40rpx;
	}

	.safeArea {
		height: 100rpx;
		width: 100%;
	}

	.title {
		margin: $marginTop 0;
		color: #333333;
		min-height: 2rem;
		font-size: 40rpx;
		display: flex;
		align-items: center;
	}

	.answer {
		// width: 100rem;
		margin: $marginTop 0;
		// height: 20rem;
		// background-color: #f2f3f5;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.answer-text {
		width: 100%;
	}

	.li {
		font-size: 36rpx;
		margin: 25rpx 0;
	}
</style>
