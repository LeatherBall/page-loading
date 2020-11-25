<template>
	<view class="loading-wrapper" :class="{'loading-wrapper-embed': embed}" :style="{'top': topSize, 'bottom': bottomSize}"
	 hover-stop-propagation @tap.stop.prevent="defThouch" @touchmove.stop.prevent="defMove">
		<view class="text-container">
			<!-- #ifdef APP-NVUE -->
			<text ref="text" class="loading-text" v-for="(item, index) in texts" :key="index" :style="{'font-size': fontSize + 'rpx'}">{{item}}</text>
			<!-- #endif -->
			<!-- #ifndef APP-NVUE -->
			<text ref="text" class="loading-text" v-for="(item, index) in texts" :key="index" :style="{'animation-delay': 102.631578 * (index + 1) + 'ms', 'font-size': fontSize + 'rpx'}" v-html="item"></text>
			<!-- #endif -->
		</view>
	</view>
</template>

<script>
	/**
	 * ls-loading 页面加载等待loading
	 * @description 通常用于页面数据需要异步加载时，为避免出现数据加载完毕之前页面上出现空数据的尴尬情况，显示loading层作为页面载入过渡
	 * @tutorial lsl
	 * @property {Boolean} nav 是否预留出标题栏的高度,通常在自定义导航栏页面中需要设置为true （默认：false）
	 * @property {Boolean} tab 是否预留出tabBar的高度 （默认：false）
	 * @property {Boolean} embed 是否为嵌入模式 （默认：false）
	 * @property {String} text 加载中的文字 （默认：正在加载）
	 * @property {String|Number} fontSize 加载中文字大小 （默认：58, 单位rpx）
	 * @example <ls-loading v-if="pageLoading"></ls-loading>
	 */
	
	// #ifdef APP-NVUE
	const animation = weex.requireModule('animation');
	// #endif
	
	export default {
		name: 'ls-loading',
		props: {
			// 是否预留出标题栏的高度
			nav: {
				type: Boolean,
				default: false
			},
			// 是否预留出tabBar的高度
			tab: {
				type: Boolean,
				default: false
			},
			// 加载中的文字
			text: {
				type: String,
				default: '正在加载'
			},
			// embed 是否为嵌入模式
			embed: {
				type: Boolean,
				default: false
			},
			// 加载中文字大小
			fontSize: {
				type: [String, Number],
				default: 58
			}
		},
		data() {
			return {
				topSize: '',
			}
		},
		computed: {
			texts() {
				// #ifdef APP-NVUE
				return this.text.split('');
				// #endif
				
				// #ifndef APP-NVUE
				return this.text.split('').map(item => {
					return item.replace(/\s/g, '&nbsp;');
				});
				// #endif
			},
			bottomSize() {
				return this.tab ? '50px' : '0';
			}
		},
		created() {
			setTimeout(() => {
				const res = uni.getSystemInfoSync();
				this.topSize = this.nav ? `${res.statusBarHeight + 44}px` : `0px`;
				// #ifdef APP-NVUE
				this.createAnimation();
				// #endif
			}, 1);
		},
		methods: {
			createAnimation() {
				const refs = this.$refs.text;
				const gap = 1200 / this.texts.length;
				const duration = 166 * this.texts.length;
				let opacity = 1;
				// 定时器执行前，先执行一次，让整个过程更流畅
				setTimeout(() => {
					for (let i = 0, length = this.texts.length; i < length; i++) {
						this.executeAnimation(refs[i], gap * (i + 1), opacity);
					}
				}, 100);
				setInterval(() => {
					opacity = opacity === 0.3 ? 1 : 0.3;
					for (let i = 0, length = this.texts.length; i < length; i++) {
						this.executeAnimation(refs[i], gap * (i + 1), opacity);
					}
				}, duration);
			},
			executeAnimation(ref, delay, opacity) {
				animation.transition(ref, {
					styles: {
						opacity: opacity,
					},
					duration: 800, //ms  
					timingFunction: 'linear',
					needLayout: false,
					delay: delay //ms  
				});
			},
			defThouch(e) {
				// 阻止点击穿透
				// #ifdef APP-NVUE
				e.stopPropagation();
				// #endif
			},
			defMove(e) {
				// 阻止滚动穿透
				// #ifdef APP-NVUE
				e.stopPropagation();
				// #endif
			}
		}
	}
</script>

<style scoped>
	.loading-wrapper {
		position: fixed;
		left: 0;
		right: 0;
		z-index: 997;
		background-color: #FFFFFF;
		/* #ifdef APP-NVUE */
		flex: 1;
		/* #endif */
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		align-items: center;
		justify-content: center;
		overflow: hidden;
	}

	.loading-wrapper-embed {
		flex: 1;
		position: relative;
		margin-top: 0;
		margin-right: 0;
		margin-bottom: 0;
		margin-left: 0;
		text-align: center;
		background-color: transparent;
		z-index: 1;
	}
	
	.text-container {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}


	.loading-text {
		font-weight: 700;
		/* #ifdef APP-NVUE */
		color: #c2c1c2;
		opacity: 0.3;
		/* #endif */
		/* #ifndef APP-NVUE */
		opacity: 1;
		color: #e8e7e8;
		animation-name: text;
		animation-timing-function: ease-in-out;
		animation-duration: 2.6s;
		animation-iteration-count: infinite;
		/* #endif */
	}
	
	/* #ifndef APP-NVUE */
	@keyframes text {
		0% {
			color: #c3c3c3;
		}
	
		20% {
			color: #e8e7e8;
		}
	
		100% {
			color: #e8e7e8;
		}
	}
	/* #endif */
</style>
