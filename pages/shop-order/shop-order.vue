<template>
	<view class="px-3 py-2">
		
		<view class="bg-white line-h p-r pb-1 mb-2 radius10 overflow-h font-24">
			<view class="py-3 px-2 line-h d-flex a-center j-sb p-r" @click="Router.navigateTo({route:{path:'/pages/shop-address/shop-address'}})">
				<view>
					<view class="pb-3 font-30">张三 <text class="color9 pl-2 font-24">156****9536</text></view>
					<view>陕西省西安市长安区韦曲街道</view>
				</view>
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
			<image src="../../static/images/my order-icon1.png" class="order-icon p-a bottom-0 left-0 right-0"></image>
		</view>
		
		
		<view >
			<view class="bg-white px-2 radius10">
				<view class="d-flex a-center j-sb py-3 bB-f5">
					<image :src="mainData.sku.mainImg[0].url" class="shopImg"></image>
					<view class="shopCon d-flex flex-column flex-1 pl-2">
						<view class="tit avoidOverflow2 pb-2 flex-1">{{mainData.title}}</view>
						<view class="font-22 color6 line-h pb-1">规格：{{mainData.sku.title}}</view>
						<view class="colorR font-32">
							<text class="price1 font-w pb-2">{{mainData.sku.o_price}}</text>/
							<text class="priceV font-w">{{mainData.sku.price}}</text>
						</view>
					</view>
				</view>
				<view class="flex py-3">
					<view class="font-26 pr-3">订单备注</view>
					<input type="text" value="" placeholder="请输入" />
				</view>
			</view>
			<view class="ggPart pb-1 flex1 px-2">
				<view class="py-3">购买数量</view>
				<view class="d-flex a-center count">
					<image src="../../static/images/shopping-icon2.png" class="count-icon1" @click="count(-1)"></image>
					<view class="num text-center f5bj">{{mainData.num}}</view>
					<image src="../../static/images/shopping-icon3.png" class="count-icon2" @click="count(1)"></image>
				</view>
			</view>
		</view>
		
		
		<view class="bg-white p-f left-0 right-0 d-flex carBot">
			<view class="font-22 d-flex a-center flex-1 px-3">总计： <text class="price1 font-30 font-w">{{totle}}</text></view>
			<view class="carBtn" @click="showToast">提交订单</view>
		</view>
		
		
		
		<!-- 提示 -->
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">订单提交成功，后期将会有专属的客服联系您的！</view>
				<view class="flex1 bT-f5 btn">
					<view class="w-50 bR-f5" @click="showToast">我知道了</view>
					<view class="colorM w-50" @click="Router.navigateTo({route:{path:'/pages/shop-index/shop-index'}})">再去逛逛</view>
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
				is_show:false,
				mainData:{},
				totle:0
			}
		},
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('buyOrder')
			console.log('buy',self.mainData)
			self.totlePrice()
		},
		onShow(){
			const self = this;
		},
		methods: {
			
			showToast(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			count(i){
				const self = this;
				if(self.mainData.num+i > 0){
					self.mainData.num += i;
				}
				self.totlePrice();
			},
			
			totlePrice(){
				const self = this;
				if(self.mainData){
					self.totle = self.mainData.num * Number(self.mainData.sku.price)
				}
				console.log('totle',self.totle)
			}
			
		}
	}
</script>
<style>
html,body{height: 100%!important;background-color: #f5f5f5;}
page{background-color: #f5f5f5;}
</style>
<style scoped>
.order-icon{width: 100%;height: 4rpx;}
input{font-size: 24rpx;flex: 1;}
.num{background-color: #fff;}
.carBot{bottom: 0;}
.tc{width: 620rpx;margin: auto;left: 0;right: 0;top: 30%;}
.tc .txt{padding: 60rpx;}
.tc .btn{line-height: 100rpx;}
</style>
