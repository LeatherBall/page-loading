<template>
	<view>
		<view class="mb">
			<view class="title">设置loading动画效果</view>
			<radio-group @change="radioChange">
				<label class="flex" v-for="(item, index) in items" :key="item.value">
					<view><radio :value="item.value" :checked="index === current" /></view>
					<view>{{ item.name }}</view>
				</label>
			</radio-group>
		</view>
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
			items: [
				{
					value: 'twinkle',
					name: '文字闪烁',
					checked: true
				},
				{
					value: 'focus',
					name: '文字聚焦'
				},
				{
					value: 'rise',
					name: '模拟水位上涨'
				},
				{
					value: 'progress',
					name: '水平进度加载'
				},
				{
					value: 'jump',
					name: '文字跳动'
				}
			],
			current: 0
		};
	},
	methods: {
		radioChange: function(evt) {
			for (let i = 0; i < this.items.length; i++) {
				if (this.items[i].value === evt.target.value) {
					this.current = i;
					break;
				}
			}
		},
		tovue() {
			uni.navigateTo({
				url: `demo-vue?animation=${this.items[this.current].value}`
			});
		},
		tonvue() {
			uni.navigateTo({
				url: `demo-nvue?animation=${this.items[this.current].value}`
			});
		}
	}
};
</script>

<style>
page {
	height: calc(100vh - 44px);
	display: flex;
	align-items: center;
	justify-content: center;
}
.title {
	font-size: 28rpx;
	font-weight: bold;
	color: #666;
}
.flex {
	display: flex;
	margin-top: 20rpx;
	font-size: 28rpx;
	color: #666;
}
.mb {
	padding-bottom: 40rpx;
	margin-bottom: 40rpx;
	border-bottom: 1rpx solid #eee;
}
</style>
