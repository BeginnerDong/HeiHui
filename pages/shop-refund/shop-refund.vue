<template>
	<view>
		
		<view class="d-flex a-start p-3 bT-f5 bg-white mb-2">
			<image src="../../static/images/my order-img.png" class="myorederImg"></image>
			<view class="myTkCon pl-2 d-flex flex-column">
				<view class="tit avoidOverflow2 pb-1">哼唱幸福 11枝粉色扶郎花束5555555555555555</view>
				<view class="font-22 color6 line-h flex-1">规格：礼盒A</view>
			</view>
		</view>
		
		<view class="d-flex bg-white p-3 mb-2">
			<view>退款原因：</view>
			<textarea value="" placeholder="请说明" />
		</view>
		
		<view class="bg-white p-3">退款金额: <text class="price1 font-32 font-w">78.8</text></view>
		
		<view class="submit Mgb colorf font-w p-f">提交</view>
		
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
</style>
<style scoped>
.myorederImg{width: 170rpx;height: 170rpx;}
.myTkCon .tit{width: 465rpx;}
.myTkCon{height: 170rpx;}
textarea{width: 540rpx;height: 120rpx;font-size: 28rpx;}
.submit{line-height: 100rpx;height: 100rpx;text-align: center;left: 0;right: 0;bottom: 0;}
</style>
