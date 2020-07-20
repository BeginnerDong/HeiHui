<template>
	<view class="h-100">
		
		<view class="h-100 d-flex p-fXY">
			<view class="left line-h h-100 font-26 color6">
				<view class="py-3 classLi" @click="changeLeft(0)" :class="leftCurr==0?'on':''">为您推荐</view>
				<view class="py-3 classLi" @click="changeLeft(1)" :class="leftCurr==1?'on':''">精选谷物</view>
				<view class="py-3 classLi" @click="changeLeft(2)" :class="leftCurr==2?'on':''">滋补山参</view>
				<view class="py-3 classLi" @click="changeLeft(3)" :class="leftCurr==3?'on':''">精品山参</view>
				<view class="py-3 classLi" @click="changeLeft(4)" :class="leftCurr==4?'on':''">养生枸杞</view>
				<view class="py-3 classLi" @click="changeLeft(5)" :class="leftCurr==5?'on':''">粮油米面</view>
				<view class="py-3 classLi" @click="changeLeft(6)" :class="leftCurr==6?'on':''">酒水饮料</view>
				<view class="py-3 classLi" @click="changeLeft(7)" :class="leftCurr==7?'on':''">精选茶叶</view>
				<view class="py-3 classLi" @click="changeLeft(8)" :class="leftCurr==8?'on':''">坚果炒货</view>
			</view>
			
			<view class="right h-100">
				<view class="radius10 bg-white mr-3 mt-3 font-26">
					<view class="px-3">
						<view class="font-30 font-w pt-5 pb-3">为您推荐</view>
						<view class="flex1 flex-wrap">
							<view class="flex4 pb-3 tjItem" @click="Router.navigateTo({route:{path:'/pages/shop-classifyGood/shop-classifyGood'}})">
								<image src="../../static/images/classification-img.png" class="classImg"></image>
								<view>小米</view>
							</view>
							<view class="flex4 pb-3 classItem">
								<image src="../../static/images/classification-img1.png" class="classImg"></image>
								<view>小米</view>
							</view>
							<view class="flex4 pb-3 classItem">
								<image src="../../static/images/classification-img2.png" class="classImg"></image>
								<view>小米</view>
							</view>
							<view class="flex4 pb-3 classItem">
								<image src="../../static/images/classification-img3.png" class="classImg"></image>
								<view>小米</view>
							</view>
						</view>
					</view>
					
					<view class="px-3">
						<view class="font-30 font-w pt-5 pb-3">精选谷物</view>
						<view class="flex1 flex-wrap">
							<view class="flex4 pb-3 tjItem">
								<image src="../../static/images/classification-img.png" class="classImg"></image>
								<view>小米</view>
							</view>
						</view>
					</view>
					<view class="px-3">
						<view class="font-30 font-w pt-5 pb-3">滋补山参</view>
						<view class="flex1 flex-wrap">
							<view class="flex4 pb-3 tjItem">
								<image src="../../static/images/classification-img.png" class="classImg"></image>
								<view>小米</view>
							</view>
						</view>
					</view>
					<view class="px-3">
						<view class="font-30 font-w pt-5 pb-3">精品山参</view>
						<view class="flex1 flex-wrap">
							<view class="flex4 pb-3 tjItem">
								<image src="../../static/images/classification-img.png" class="classImg"></image>
								<view>小米</view>
							</view>
						</view>
					</view>
					
				</view>
			</view>
			
		</view>
		
		
		
		
		
		<!-- <view style="height: 140rpx;"></view> -->
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/shop-classify/shop-classify'}})">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
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
				Router:this.$Router,
				leftCurr:0
			}
		},
		methods: {
			changeLeft(i){
				const self = this;
				self.leftCurr = i
			}
		}
	}
</script>

<style>
html{height: 100%!important;}
page{height: 100%;background-color: #f5f5f5;}

.left{width: 180rpx;padding-bottom: 100rpx;box-sizing: border-box;overflow-y: auto;}
.left .classLi{text-align: center;margin-bottom: 40rpx;}
.left .on{color: #E39423;position: relative;}
.left .on::before{content: ''; height: 60rpx;width: 14rpx;border-radius: 7rpx;background-color: #E39423;position: absolute;left: -7rpx;top: 14rpx;}
.right{height: 100%;overflow-y: auto;box-sizing: border-box;padding-bottom: 140rpx;}

.classImg{width: 120rpx;height: 120rpx;}
.classItem{width: 33.33%;}
</style>
