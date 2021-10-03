<template>
	<view class="container">
		<!-- 搜索区域 -->
		<view class="search_area">
			<input type="text" placeholder="请输入标题关键词" @input="changeEvent" class="input" v-model="searchValue" />
		</view>
		<view class="searchRes">
		<!-- 搜索结果：搜索框有值时，展示 -->
			<l-cell-group title="搜索结果" v-show="isShowSearchRes && isHasSearchValue">
				<view v-show="showList.length===0" class="null-data">暂无数据</view>
				<!-- isBorderTop 首行为true，其他为false -->
				<l-cell-item @click="confirmObj(item)" :title="item.question" :isBorderTop="index===0" v-for="(item, index) of showList" :key="index"></l-cell-item>
			</l-cell-group>
		</view>
		<!-- 详细内容：搜索框无值时，展示 -->
		<view class="resultShow" v-show="!isShowSearchRes">
			<view class="title">
				{{showObj.question}}
			</view>
			<!-- 图 -->
			<view class="answer" v-show="showObj.imgSrc">
				<!-- aspectFit: 不缩放，保证长边可完全显示 -->
				<image mode="aspectFit" :src="showObj.imgSrc" @click="previewImage(showObj.imgSrc)"></image>
			</view>
			<!-- 答案区域-文字 -->
			<view class="answer-text">
				<ul>
					<li v-for="(item,index) in showObj.answerText" class="li">{{item}}</li>
				</ul>
			</view>
		</view>
		<!-- 重置当前搜索框 -->
		<l-absolute-btn bgColor="#96bf96" imgSrc="reset" @click="resetSearchValue" bottom="100px"></l-absolute-btn>
		<!-- 详细内容：搜索框有值时，不展示 -->
		<view class="safeArea"></view>
	</view>
</template>

<script>
	import questionList from '../../common/questionList.js'
	import zongheList from '../../common/zongheList.js'
	export default {
		data() {
			return {
				searchValue: '',
				// 汇总数组（包括理论背诵点 & 综合知识）
				allList: JSON.parse(JSON.stringify(questionList)).concat(JSON.parse(JSON.stringify(zongheList))),
				timer: null,
				// 搜索得到的结果的数组
				showList:[],
				// 详细结果
				showObj:{},
				// 是否显示搜索结果
				isShowSearchRes:false
			};
		},
		computed:{
			// 搜索框是否有值
			isHasSearchValue(){
				if(this.searchValue !== ''){
					return true
				}else{
					this.isShowSearchRes = false
					return false
				}
			}
		},
		methods: {
			// 预览图片
			previewImage(src) {
				uni.previewImage({
					urls: [src]
				})
			},
			changeEvent(e) {
				// 防抖函数
				if (this.timer) {
					clearTimeout(this.timer)
					// 在搜索框有值变化时，清空搜索结果数组，避免闪屏
					this.showList = []
				}
				this.timer = setTimeout(() => {
					this.timer = null
					this.renderData(e.detail.value)
				}, 300)
			},
			renderData(value){
				if(value === ''){
					return []
				}
				this.isShowSearchRes = true
				this.showList = this.allList.filter(item => {
					return item.question.includes(value)
				})
			},
			// 确认选择搜索结果
			confirmObj(obj){
				this.showObj = obj
				this.isShowSearchRes = false
			},
			resetSearchValue(){
				this.searchValue = ''
			}
		}
	}
</script>

<style lang="scss" scoped>
	$tabBarHeight: 70px;
	$navigateHeight: 44px;
	$margin: 0 20px;
	$marginTop: 40rpx;

	.container {
		// background-color: #C8C7CC;
		// height: calc(100vh - #{$tabBarHeight} - #{$navigateHeight});
		// height: var(--status-bar-height);
		width: 100%;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.input {
		// margin: $margin;
		padding: 0 15px;
		border-radius: 10px;
		height: 40px;
		width: 100%;
		// border: 1px solid #F1F1F1;
		background-color: #F1F1F1;
	}
	
	.null-data{
		display: flex;
		justify-content: center;
		color: #f0ad4e;
	}

	.search_area {
		position: sticky;
		z-index: 9999;
		top: 0;
		height: 80rpx;
		margin-top: 15px;
		width: 90%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		// background-color: #999999;
	}

	.searchRes {
		// height: 150rpx;
		width: 90%;
		// background-color: #2C405A;
	}

	.safeArea {
		height: 100rpx;
		width: 100%;
	}
	
	// 详细结果
	.resultShow{
		width: 90%;
	}
	.title {
		margin-top: $marginTop;
		color: #333333;
		min-height: 2rem;
		font-size: 40rpx;
		display: flex;
		align-items: center;
		font-weight: bold;
	}
	
	.answer {
		// width: 100rem;
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
