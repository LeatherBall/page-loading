<template>
	<view>
		<button type="primary" @tap="tovue">去vue页</button>
		<!-- #ifdef APP-PLUS -->
		<button type="primary" @tap="tonvue" style="margin-top: 20rpx;">去nvue页</button>
		<!-- #endif -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		methods: {
			tovue() {
				uni.navigateTo({
					url: 'index3'
				})
			},
			tonvue() {
				uni.navigateTo({
					url: 'index2'
				})
			}
		}
	}
</script>

<style>
	page {
		height: calc(100vh - 44px);
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
