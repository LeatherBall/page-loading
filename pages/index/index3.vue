<template>
	<view>
		<ls-loading :nav="false" text="now loading..." v-if="pageLoading"></ls-loading>
		<!-- 下面照常写你的页面布局 -->
		<text>我是vue页面</text>
	</view>
</template>

<script>
	import lsLoading from '@/components/ls-loading/ls-loading.vue';
	export default {
		components: {
			lsLoading
		},
		data() {
			return {
				pageLoading: true,
			}
		},
		onLoad() {
			// 这里模拟接口请求,接口请求成功后将pageLoading改为false
			setTimeout(() => {
				this.pageLoading = false;
			}, 5000);
		}
	}
</script>

<style>
	
</style>
