<template>
	<view>
		
		<!-- banner -->
		<view class="banner radius20 overflow-h mx-3 mt-2"  @click="Router.redirectTo({route:{path:'/pages/shop-classify/shop-classify'}})">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#FF6F48">
				<block>
					<swiper-item class="swiper-item">
						<image src="../../static/images/mall-banner.png" class="slide-image" />
					</swiper-item>
				</block>
				<block>
					<swiper-item class="swiper-item">
						<image src="../../static/images/mall-banner.png" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<!-- 金刚区 -->
		<view class="flex1 mx-3 font-24 bg-white mt-2 radius10 pb-4">
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img.png" ></image>
				<view>精选谷物</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img1.png" ></image>
				<view>滋补山参</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img2.png" ></image>
				<view>精品山珍</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img3.png" ></image>
				<view>养生枸杞</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img4.png" ></image>
				<view>粮油米面</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img5.png" ></image>
				<view>就睡饮料</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img6.png" ></image>
				<view>精选茶叶</view>
			</view>
			<view class="tt flex4 pt-4">
				<image src="../../static/images/mall-img7.png" ></image>
				<view>坚果炒货</view>
			</view>
		</view>
		
		<!-- 热门活动 -->
		<view class="mx-3 font-26 bg-white mt-2 radius10 p-3 pb-0">
			<view class="flex1 pb-3"> 
				<view class="flex font-34 font-w">
					<image src="../../static/images/mall-icon.png" class="mall-icon"></image>
					<view>热门活动</view>
				</view>
				<view class="flex font-24 color9">
					<view>更多</view>
					<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
				</view>
			</view>
			<view class="shopHot font-32 line-h flex1">
				<view class="pb-3">
					<image src="../../static/images/mall-img8.png" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">兰芝（LANZHI 52365）</view>
					<view class="price1 font-w pb-2">78.8</view>
					<view class="priceV font-w">68.8</view>
				</view>
			</view>
		</view>
		
		<!-- 精品推荐 -->
		<view class="mx-3 font-26 bg-white mt-2 radius10 p-3 pb-0">
			<view class="flex1 pb-3"> 
				<view class="flex font-34 font-w">
					<image src="../../static/images/mall-icon1.png" class="mall-icon"></image>
					<view>精品推荐</view>
				</view>
				<view class="flex font-24 color9">
					<view>更多</view>
					<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
				</view>
			</view>
			<view class="shopJp font-32 line-h flex1">
				<view class="pb-3">
					<image src="../../static/images/mall-img9.png" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">43度飞天茅台 1.3L/瓶</view>
					<view class="colorR">
						<text class="price1 font-w pb-2">78.8</text>/
						<text class="priceV font-w">68.8</text>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 为你推荐 -->
		<view class="flex0 pt-4">
			<view class="line100"></view>
			<view class="flex font-34 font-w px-3">
				<image src="../../static/images/mall-icon2.png" class="mall-icon"></image>
				<view>为你推荐</view>
			</view>
			<view class="line100"></view>
		</view>
		<view class="flex1 px-3">
			<view class="radius10 overflow-h pt-3 shopTj">
				<image src="../../static/images/mall-img10.png" mode=""></image>
				<view class="bg-white p-2">
					<view class="tit avoidOverflow2 font-26">乔府大院五常核心产区留胚鲜米品鉴装</view>
					<view class="font-20 color9 line-h py-2">销量 235</view>
					<view class="colorR font-32">
						<text class="price1 font-w pb-2">78.8</text>/
						<text class="priceV font-w">68.8</text>
					</view>
				</view>
			</view>
		</view>
		
		
		
		<view style="height: 140rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-classify/shop-classify'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>分类</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-car/shop-car'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>购物车</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-user/shop-user'}})">
				<image src="../../static/images/nabar4.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.banner image{height: 280rpx;border-radius: 10rpx;}
.tt{width: 25%;}
.tt image{width: 90rpx;height: 90rpx;margin-bottom: 20rpx}

.mall-icon{width: 44rpx;height: 43rpx;margin-right: 10rpx;}
.shopHot image{width: 190rpx;height: 190rpx;}
.shopHot .tit{width: 190rpx;}
.shopJp image{width: 300rpx;height: 200rpx;}
.shopJp .tit{width: 300rpx;}
.line100{width: 100rpx;border-top: 1px solid #e1e1e1;}
.shopTj{width: 330rpx;}
.shopTj image{width: 100%;height: 300rpx;}
.shopTj .tit{width: 290rpx;}
</style>
