<template>
	<view>
		
		<view class="top px-3 py-4 Mgb flex font-36 colorf font-w">
			<!-- <image src="../../static/images/the order details-icon.png" class="order-icon1"></image> -->
			<!-- <image src="../../static/images/the order details-icon1.png" class="order-icon1"></image> -->
			<!-- <image src="../../static/images/the order details-icon2.png" class="order-icon1"></image> -->
			<image src="../../static/images/the order details-icon3.png" class="order-icon2"></image>
			<!-- <view>待发货</view> -->
			<!-- <view>已发货</view> -->
			<!-- <view>待发货</view> -->
			<view>已退款</view>
		</view>
		
		<view class="d-flex p-3 line-h bg-white mb-2">
			<image src="../../static/images/the order details-icon4.png"  class="order-icon3"></image>
			<view>
				<view class="font-w pb-3"><text class="pr-3">张梦雨</text>18895864885</view>
				<view class="color6">陕西省西安市雁塔区高西安大都荟</view>
			</view>
		</view>
		
		<view class="px-3 bg-white mb-2">
			<view class="d-flex a-start j-sb py-3 bB-f5">
				<image src="../../static/images/my order-img.png" class="myorederImg"></image>
				<view class="myCon pl-2 flex-1">
					<view class="tit avoidOverflow2 pb-1">哼唱幸福 11枝粉色扶郎花束6544156</view>
					<view class="font-22 color6 line-h flex-1">规格：礼盒A</view>
				</view>
				<view>
					<view class="price1">78.8</view>
					<view class="font-22 color9 text-right pt-1">x1</view>
				</view>
			</view>
			<view class="d-flex a-start j-sb py-3">
				<image src="../../static/images/my order-img.png" class="myorederImg"></image>
				<view class="myCon pl-2 flex-1">
					<view class="tit avoidOverflow2 pb-1">哼唱幸福 11枝粉色扶郎花束6544156</view>
					<view class="font-22 color6 line-h flex-1">规格：礼盒A</view>
				</view>
				<view>
					<view class="price1">78.8</view>
					<view class="font-22 color9 text-right pt-1">x1</view>
				</view>
			</view>
		</view>
		
		<view class="bg-white px-3 mb-2">
			<view class="flex1 py-3 bB-f5">
				<view>商品总额</view>
				<view class="price1 font-w">78.8</view>
			</view>
			<view class="text-right py-3 sfk">实付款：<text class="price1 font-w font-36">78.8</text></view>
		</view>
		
		<view class="bg-white p-3 font-24 color6 line-h">
			<view class="pb-3">退款原因：不喜欢/不想要</view>
			<view class="pb-3">订单编号：sdferfb5569852</view>
			<view class="pb-3">下单时间：2020-06-23 09:00:08</view>
			<view>付款类型：微信付款</view>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.order-icon1{width: 78rpx;height: 70rpx;margin-right: 30rpx;}
.order-icon2{width: 70rpx;height: 70rpx;margin-right: 30rpx;}
.order-icon3{width: 38rpx;height: 38rpx;margin-right: 20rpx;}
.myorederImg{width: 173rpx;height: 173rpx;}
.price1{color: #222;}
.myCon .tit{width: 335rpx;}
.sfk .price1{color: #E80F0F;}
</style>
