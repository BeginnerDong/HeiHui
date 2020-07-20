<template>
	<view>
		
		<view class="list flexX p-fXY bg-white shadow">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待发货</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">待收货</view>
			<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">已完成</view>
			<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">已退款</view>
		</view>
		
		<view style="height: 120rpx;"></view>
		<view class="bg-white mx-3 radius10 p-2 mb-2 oredrBox">
			<view class="flex1 pb-3 font-24">
				<view class="flex">
					<image src="../../static/images/my order-icon.png" class="mai-icon"></image>
					<view>2020-6-29 10:20:24</view>
				</view>
				<view class="colorR">待发货</view>
			</view>
			<!-- 全部、待发货、待收货、已完成展示 -->
			<view @click="Router.navigateTo({route:{path:'/pages/shop-orderDetail/shop-orderDetail'}})">
				<view class="d-flex a-start j-sb pt-3 bT-f5 order">
					<image src="../../static/images/my order-img.png" class="myorederImg"></image>
					<view class="myCon pl-2">
						<view class="tit avoidOverflow2 pb-1">哼唱幸福 11枝粉色扶郎花束</view>
						<view class="font-22 color6 line-h flex-1">规格：礼盒A</view>
					</view>
					<view>
						<view class="price1">78.8</view>
						<view class="font-22 color9 text-right pt-1">x1</view>
					</view>
				</view>
				<view class="color9 text-right pb-3">实付款：<text class="price1 font-w">78.8</text></view>
			</view>
			
			<view class="d-flex j-end">
				<view class="tkBtn b-e1 font-24" @click="Router.navigateTo({route:{path:'/pages/shop-refund/shop-refund'}})">申请退款</view>
				<!-- <view class="tkBtn borderM font-24 colorM">确认收货</view> -->
			</view>
			
			<!-- 已退款展示 -->
			<!-- <view>
				<view class="d-flex a-start j-sb pt-3 bT-f5 order">
					<image src="../../static/images/my order-img.png" class="myorederImg"></image>
					<view class="myTkCon pl-2 d-flex flex-column">
						<view class="tit avoidOverflow2 pb-1">哼唱幸福 11枝粉色扶郎花束5555555555555555</view>
						<view class="font-22 color6 line-h flex-1">规格：礼盒A</view>
						<view class="font-24">退款金额: <text class="price1 font-32">78.8</text></view>
					</view>
				</view>
				<view class="font-24 color9 pt-3">
					<view>收件人：<text class="font-28 color2 px-1">张萌</text> 15665988946</view>
					<view>申请时间：2020-6-29 10:20:24</view>
				</view>
			</view> -->
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0
			}
		},
		onLoad(option){
			const self = this;
			self.liCurr = option.id;
		},
		methods: {
			changeLi(i){
				const self = this;
				self.liCurr = i;
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.list{height: 100rpx;z-index: 100;}
.li{width: 20%;}
.mai-icon{width: 32rpx;height: 32rpx;margin-right: 10rpx;}
.myorederImg{width: 170rpx;height: 170rpx;}
.myCon .tit{width: 334rpx;}
.myCon .price1{color: #222;font-size: 32rpx;}
.tkBtn{line-height: 60rpx;width: 150rpx;text-align: center;border-radius: 30rpx;}
.myTkCon .tit{width: 465rpx;}
.myTkCon{height: 170rpx;}
</style>
