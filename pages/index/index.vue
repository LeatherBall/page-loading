<template>
	<view>
		<button type="primary" @tap="tovue">去vue页</button>
		<button type="primary" @tap="tonvue" style="margin-top: 20rpx;">去nvue页</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		onLoad() {
			// 这里模拟接口请求,接口请求成功后将pageLoading改为false
			setTimeout(() => {
				// this.pageLoading = false;
			}, 1000);
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
