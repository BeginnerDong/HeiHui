<template>
	<view>
		
		<image src="../../static/images/jianjie-img.png" class="img"></image>
		
		<view class="p-r item">
			<!-- <image src="../../static/images/jianjie-img4.png" class="bg-txt"></image> -->
			<view class="flex0 pt-5 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">公司简介</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 txt">
				公司全称 深圳前海恒辉资产管理有限公司 英文名称 Shenzhen Qianhai Eternal Glory As... 壹财富是国内领先的金融科技财富管理平台,云集信托资管、私募基金、海外资产等优质...<br /><br />公司全称 深圳前海恒辉资产管理有限公司 英文名称 Shenzhen Qianhai Eternal Glory As... 壹财富是国内领先的金融科技财富管理平台,云集信托资管、私募基金、海外资产等优质...
			</view>
		</view>
		
		
		<view class="p-r item">
			<view class="flex0 pt-3 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">企业文化</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 txt">
				公司全称 深圳前海恒辉资产管理有限公司 英文名称 Shenzhen Qianhai Eternal Glory As... 壹财富是国内领先的金融科技财富管理平台,云集信托资管、私募基金、海外资产等优质...<br /><br />公司全称 深圳前海恒辉资产管理有限公司 英文名称 Shenzhen Qianhai Eternal Glory As... 壹财富是国内领先的金融科技财富管理平台,云集信托资管、私募基金、海外资产等优质...
				<image src="../../static/images/jianjie-img1.png" class="img1"></image>
			</view>
		</view>
		
		<view class="p-r item">
			<view class="flex0 pt-3 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">合作伙伴</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 flex1">
				<image src="../../static/images/jianjie-img3.png" class="img2"></image>
			</view>
		</view>
		
		<view class="p-r item">
			<view class="flex0 pt-3 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">联系我们</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 contact">
				<view class="flex bB-f5 py-2">
					<image src="../../static/images/jianjie-icon4.png" class="con-icon"></image>
					<input type="text" value="" placeholder="姓名" />
				</view>
				<view class="flex bB-f5 py-2 mt-3">
					<image src="../../static/images/jianjie-icon3.png" class="con-icon1"></image>
					<input type="text" value="" placeholder="手机号" />
				</view>
				<view class="flex bB-f5 py-2 mt-3">
					<image src="../../static/images/jianjie-icon2.png" class="con-icon2"></image>
					<input type="text" value="" placeholder="留言框" />
				</view>
				<view class="btn600">确定</view>
			</view>
		</view>
		
		<image src="../../static/images/jianjie-img2.png" class="img3"></image>
		
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
.img{height: 428rpx;width: 100%;}
.bg-txt{width: 690rpx;height: 468rpx;position: absolute;top: 70rpx;;left: 0;right: 0;margin: 0 auto;}
.item{margin-bottom: 30rpx;}
.img1{width: 600rpx;height: 340rpx;margin: 30rpx auto 0;}
.img2{width: 200rpx;height: 130rpx;margin-bottom: 20rpx;}
.img3{width: 100%;height: 200rpx;margin-top: 100rpx;}
.con-icon{width: 26rpx;height: 30rpx;}
.con-icon1{width: 23rpx;height: 30rpx;}
.con-icon2{width: 28rpx;height: 28rpx;}
.contact input{font-size: 26rpx;color: #666;padding-left: 30rpx;flex: 1;}
.btn600{width: 630rpx;margin-top: 50rpx;}
</style>
