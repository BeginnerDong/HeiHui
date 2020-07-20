<template>
	<view>
		
		<view class="p-r">
			<image src="../../static/images/my-img1.png" class="myBg"></image>
			<view class="flex1 h-100 px-3 p-r pt-5">
				<image src="../../static/images/my-img.png" class="userImg"></image>
				<view class="flex-1 font-32 pl-2 colorf">哆啦A梦</view>
			</view>
		</view>
		
		<view class="p-r bg-white mx-3 mt-3 radius10 py-4 px-2 shadow">
			<view class="flex1 pb-3">
				<view class="flex font-32 font-w">我的订单</view>
				<view class="flex font-22 color9" @click="Router.navigateTo({route:{path:'/pages/shop-userOrder/shop-userOrder?id=0'}})">
					<view>查看</view>
					<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
				</view>
			</view>
			<view class="font-24 color6 line-h flex1 px-2">
				<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/shop-userOrder/shop-userOrder?id=0'}})">
					<image src="../../static/images/my-icon.png" class="my-icon"></image>
					<view class="pt-2">全部</view>
				</view>
				<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/shop-userOrder/shop-userOrder?id=1'}})">
					<image src="../../static/images/my-icon1.png" class="my-icon1"></image>
					<view class="pt-2">待发货</view>
				</view>
				<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/shop-userOrder/shop-userOrder?id=2'}})">
					<image src="../../static/images/my-icon2.png" class="my-icon2"></image>
					<view class="pt-2">待收货</view>
				</view>
				<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/shop-userOrder/shop-userOrder?id=3'}})">
					<image src="../../static/images/my-icon3.png" class="my-icon3"></image>
					<view class="pt-2">已收货</view>
				</view>
				<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/shop-userOrder/shop-userOrder?id=4'}})">
					<image src="../../static/images/my-icon4.png" class="my-icon3"></image>
					<view class="pt-2">已退款</view>
				</view>
			</view>
		</view>
		
		<view class="p-r bg-white mx-3 mt-3 radius10 py-4 px-2 shadow">
			<view class="flex font-32 font-w pb-3">其他功能</view>
			<view class="font-24 line-h flex">
				<view class="flex4 w-25"  @click="Router.navigateTo({route:{path:'/pages/shop-address/shop-address'}})">
					<image src="../../static/images/my-icon5.png" class="my-icon"></image>
					<view class="pt-2">收货地址</view>
				</view>
				<view class="flex4 w-25" @click="Router.navigateTo({route:{path:'/pages/shop-card/shop-card'}})">
					<image src="../../static/images/my-icon6.png" class="my-icon2"></image>
					<view class="pt-2">我的卡券</view>
				</view>
			</view>
		</view>
		
		
		
		
		
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
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
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/shop-user/shop-user'}})">
				<image src="../../static/images/nabar4-a.png" mode=""></image>
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
.myBg{width: 750rpx;height: 260rpx;position: absolute;top: 0;left: 0;right: 0;}
.my-icon{width: 42rpx;height: 51rpx;}
.my-icon1{width: 48rpx;height: 45rpx;}
.my-icon2{width: 54rpx;height: 46rpx;}
.my-icon3{width: 46rpx;height: 46rpx;}
.w-25{width: 25%;}
</style>
