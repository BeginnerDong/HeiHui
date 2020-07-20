<template>
	<view class="px-3">
		
		<view class="map my-2 shadowM">
			<image src="../../static/images/contactus-img.png" mode=""></image>
		</view>
		
		<view class="flex1 py-3 pl-4 shadowM line-h mb-2">
			<image src="../../static/images/contactus-icon.png" class="us-icon"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">联系电话</view>
				<view>029-2536-4868</view>
			</view>
		</view>
		<view class="flex1 py-3 pl-4 shadowM line-h mb-2">
			<image src="../../static/images/contactus-icon1.png" class="us-icon"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">联系地址</view>
				<view>陕西省西安市雁塔区高新大都荟A座2007</view>
			</view>
		</view>
		<view class="flex3 py-3 pl-4 shadowM line-h mb-2">
			<image src="../../static/images/contactus-icon2.png" class="us-icon1"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">意见反馈</view>
				<textarea value="" placeholder="请输入" />
				<view class="btn540 mt-2">提交</view>
			</view>
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

<style scoped>
.map{height: 300rpx;width: 690rpx;}
.map image{width: 100%;height: 100%;}
textarea{width: 540rpx;height: 240rpx;background-color: #f5f5f5;font-size: 22rpx;padding: 20rpx;box-sizing: border-box;margin: 0;}
</style>
