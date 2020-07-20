<template>
	<view class="line-h">
		
		<!-- banner -->
		<view class="banner p-r">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#FF6F48">
				<block>
					<swiper-item class="swiper-item">
						<image src="../../static/images/goods details-img.png" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<view class="bg-white p-3 mb-2">
			<view class="font-32 pb-4 font-w">LOVE礼盒系列·生生不息的爱</view>
			<view class="flex1">
				<view class="colorR font-32">
					<text class="price1 font-w pb-2">78.8</text>/
					<text class="priceV font-w">68.8</text>
				</view>
				<view class="font-22 color9">销量 255</view>
			</view>
		</view>
		
		<view class="px-3 py-4 flex1 bg-white mb-2">
			<view class="font-w">规格</view>
			<view class="font-24 flex-1 d-flex j-sb pl-3" @click="ggShow">
				<view class="flex-1 color9">请选择</view>
				<image src="../../static/images/goods details-icon.png" class="gg-icon"></image>
			</view>
		</view>
		
		<view class="py-4 flex1 bg-white mb-2">
			<view class="px-3 font-w">详情描述</view>
			<view>
				<image src="../../static/images/goods details-img2.png" class="detailImg"></image>
			</view>
		</view>
		
		<view style="height: 100rpx;"></view>
		<view class="bg-white text-center p-f bottom-0 left-0 right-0 flex1 bT-f5 px-3">
			<view class="" @click="Router.navigateTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/goods details-icon1.png" class="fh-icon"></image>
				<view class="font-20 pt-1">返回</view>
			</view>
			<view class="" @click="Router.navigateTo({route:{path:'/pages/shop-car/shop-car'}})">
				<image src="../../static/images/goods details-icon2.png" class="car-icon"></image>
				<view class="font-20 pt-1">购物车</view>
			</view>
			<view class="flex1 btnBox font-w colorf">
				<view class="btn">加入购物车</view>
				<view class="btn">立即购买</view>
			</view>
		</view>
		
		
		<!-- 规格 -->
		<view class="bg-mask line-h" v-show="gg_show">
			<view class="radius20-T bg-white p-a bottom-0 left-0 right-0 pt-3 d-flex flex-column ggBox">
				<view class="flex a-end pb-3 px-3">
					<view class="gg"><image src="../../static/images/goods details-img.png"></image></view>
					<view class="pl-2 flex-1">
						<view class="font-32 pb-3 colorR">
							<text class="price1 font-w pb-2">78.8</text>/
							<text class="priceV font-w">68.8</text>
						</view>
						<view class="font-24 color9">请选择规格</view>
					</view>
					<view @click="ggShow">
						<image src="../../static/images/goods details-icon3.png" class="x-icon"></image>
					</view>
				</view>
				
				<view class="flex-1 flexY flex-column mb-3 px-3 ggPartBox">
					<view class="ggPart pb-1">
						<view class="py-3 font-w">颜色</view>
						<view class="font-24 color6">
							<view class="span on">土豪金</view>
							<view class="span">白色</view>
							<view class="span">黑色</view>
							<view class="span">太空银色</view>
						</view>
					</view>
					<view class="ggPart pb-1 flex1">
						<view class="py-3 font-w">购买数量</view>
						<view class="d-flex a-center count">
							<image src="../../static/images/shopping-icon2.png" class="count-icon1"></image>
							<view class="num text-center f5bj">1</view>
							<image src="../../static/images/shopping-icon3.png" class="count-icon2"></image>
						</view>
					</view>
				</view>
				
				<view class="font-30 colorf text-center flex1 bT-e1 px-3 p-a left-0 right-0 bottom-0">
					<view class="ggBtn">加入购物车</view>
					<view class="ggBtn" @click="Router.navigateTo({route:{path:'/pages/shop-order/shop-order'}})">立即购买</view>
				</view>
			</view>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				gg_show:false
			}
		},
		methods: {
			ggShow(){
				const self = this;
				self.gg_show = !self.gg_show
			}
		}
	}
</script>
<style>
	page{background-color: #f5f5f5;}
</style>
<style scoped>
.banner .swiper-box{width: 750rpx;height: 750rpx;}
.banner image{width: 750rpx;height: 750rpx;}

.gg-icon{width: 38rpx;height: 10rpx;}
.detailImg{width: 750rpx;height: 3950rpx;}

.fh-icon{width: 41rpx;height: 33rpx;margin: auto;}
.car-icon{width: 38rpx;height: 38rpx;margin: auto;}
.btnBox{width: 460rpx;}
.btn{width: 220rpx;line-height: 80rpx;background-color: #6F7178;margin: 20rpx 0;border-radius: 40rpx;}
.btn:nth-child(2){background-color: #F59824;}

.ggBox{min-height: 900rpx;}
.gg{width: 182rpx;}
.gg image{width: 182rpx;height: 182rpx;border-radius: 10rpx;}
.span{background-color: #f5f5f5;padding: 0 20rpx;margin-right: 30rpx;margin-bottom: 20rpx;display: inline-block;line-height: 56rpx;border-radius: 28rpx;}
.ggPart .on{background-color: #FFF7ED;color: #E39423;border: 1px solid #E39423;}
.ggBtn{line-height: 80rpx;background-color: #6F7178;width: 330rpx;border-radius: 40rpx;margin: 20rpx 0;}
.ggBtn:nth-child(2){background-color: #F59824;}
.ggPartBox{max-height: 600rpx;}
</style>
