<template>
	<view>
		<view class="add px-3 font-28 mb-4">
			<view class="flex1 py-4 bB-f5">
				<view>姓名</view>
				<input type="text" value="" placeholder="填写姓名" />
			</view>
			<view class="flex1 py-4 bB-f5">
				<view>手机号</view>
				<input type="text" value="" placeholder="填写手机号" />
			</view>
			<view class="flex1 py-4 bB-f5">
				<view>所在地区</view>
				<view class="flex1 font-22 color9">请选择<image src="../../static/images/mall-icon3.png" class="R-icon"></image></view>
			</view>
			<view class="flex1 py-4 bB-f5">
				<view>详细地址</view>
				<input type="text" value="" placeholder="请填写" />
			</view>
			<view class="flex1 py-4 bB-f5">
				<view>设为默认地址</view>
				<!-- <image src="../../static/images/address-icon2.png" class="add-icon2"></image> -->
				<image src="../../static/images/address-icon1.png" class="add-icon2"></image>
			</view>
		</view>
		
		<view class="btn690 Mgb colorf">确定</view>
		<view class="btn690 f5bj" v-show="type">删除地址</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				type:''
			}
		},
		onLoad(options){
			const self = this;
			if(options.type){
				self.type = options.type
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.add input{text-align: right;font-size: 22rpx;flex: 1;}
.add-icon2{width: 67rpx;height: 34rpx;}
.btn690{text-align: center;font-size: 30rpx;line-height: 80rpx;border-radius: 10rpx;margin: 0 30rpx 20rpx;width: 690rpx;}
</style>
