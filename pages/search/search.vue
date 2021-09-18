<template>
	<view class="container">
		search
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss" scoped>
	$tabBarHeight: 70px;
	$navigateHeight: 44px;
.container{
	background-color: #C8C7CC;
	height: calc(100vh - #{$tabBarHeight} - #{$navigateHeight});
	// height: var(--status-bar-height);
	width: 100%;
}
</style>
