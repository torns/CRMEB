<template>
	<!-- #ifdef H5 -->
	<view v-if="isShow" class="header">
		<view class="serch-wrapper acea-row row-middle">
			<view class="logo">
				<image :src="logoConfig" mode="heightFix"></image>
			</view>
			<view class="input acea-row row-middle fillet" hover-class="none" @click="goPage"><text class="iconfont icon-sousuo"></text>
				搜索商品</view>
		</view>
	</view>
	<view v-else-if="isIframe" class="header">
		<view class="serch-wrapper acea-row row-middle">
			<view class="logo">
				<image :src="logoConfig" mode="heightFix"></image>
			</view>
			<view class="input acea-row row-middle fillet" hover-class="none" @click="goPage"><text class="iconfont icon-sousuo"></text>
				搜索商品</view>
		</view>
	</view>
	<!-- #endif -->
	<!-- #ifdef MP -->
	<view v-if="isShow">
		<view class="mp-header">
			<view class="sys-head" :style="{height:sysHeight}"></view>
			<view class="serch-box" style="height: 43px;">
				<view class="serch-wrapper acea-row row-middle">
					<view class="logo">
						<image :src="logoConfig" mode="heightFix"></image>
					</view>
					<navigator url="/pages/goods_search/index" class="input acea-row row-middle fillet" hover-class="none"><text class="iconfont icon-sousuo"></text>
						搜索商品</navigator>
				</view>
			</view>
		</view>
	</view>
	<!-- #endif -->
</template>

<script>
	let app = getApp();
	let statusBarHeight = uni.getSystemInfoSync().statusBarHeight + 'px';
	import {
		goPage
	} from '@/libs/order.js'
	export default {
		name: 'headerSerch',
		props: {
			dataConfig: {
				type: Object,
				default: () => {}
			}
		},
		data() {
			return {
				logoConfig: '',
				hotWords: [],
				sysHeight: statusBarHeight,
				name: this.$options.name,
				isShow: true,
				isIframe: app.globalData.isIframe
			};
		},
		watch: {
			dataConfig: {
				immediate: true,
				handler(nVal, oVal) {
					if(nVal){
						this.logoConfig = nVal?nVal.imgUrl.url:'';
						this.hotWords = nVal.hotList.list
						this.isShow = nVal.isShow.val
						uni.setStorageSync('hotList',this.hotWords);
					}
				}
			}
		},
		mounted() {
			let that = this;
			// #ifdef MP
			this.$nextTick(function() {
				// 获取小程序头部高度
				let info = uni.createSelectorQuery().in(this).select(".mp-header");
				info.boundingClientRect(function(data) {
					that.marTop = data.height
				}).exec()
			})
			// #endif
		},
		methods: {
			goPage() {
				goPage().then(res => {
					uni.navigateTo({
						url: '/pages/goods_search/index'
					})
				})
			}
		}
	}
</script>

<style lang="scss">
	/* #ifdef H5 */
	.header {
		width: 100%;
		height: 100rpx;
		background: #fff;

		.serch-wrapper {
			padding: 20rpx 50rpx 0 53rpx;

			.logo {
				height: 42rpx;
				margin-right: 30rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.input {
				height: 58rpx;
				padding: 0 0 0 30rpx;
				background: rgba(247, 247, 247, 1);
				border: 1px solid rgba(241, 241, 241, 1);
				color: #999;
				font-size: 28rpx;
				flex: 1;

				.iconfont {
					margin-right: 20rpx;
					color: #555555;
				}

				// 没有logo，直接搜索框
				&.on {
					width: 100%;
				}

				// 设置圆角
				&.fillet {
					border-radius: 29rpx;
				}

				// 文本框文字居中
				&.row-center {
					padding: 0;
				}
			}
		}
	}

	/* #endif */
	/* #ifdef MP */
	.mp-header {
		z-index: 999;
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		background: #fff;

		.serch-wrapper {
			height: 100%;
			padding: 0 220rpx 0 53rpx;

			.logo {
				height: 42rpx;
				margin-right: 30rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.input {
				height: 50rpx;
				padding: 0 0 0 30rpx;
				background: rgba(247, 247, 247, 1);
				border: 1px solid rgba(241, 241, 241, 1);
				color: #999;
				font-size: 28rpx;
				flex: 1;

				.iconfont {
					margin-right: 20rpx;
					color: #555555;
				}

				// 没有logo，直接搜索框
				&.on {
					width: 70%;
				}

				// 设置圆角
				&.fillet {
					border-radius: 29rpx;
				}

				// 文本框文字居中
				&.row-center {
					padding: 0;
				}
			}
		}
	}

	/* #endif */
</style>
