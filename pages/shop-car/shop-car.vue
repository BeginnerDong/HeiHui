<template>
	<view>
		
		<view class="d-flex j-sb p-3 bB-f5">
			<view>全部商品({{mainData.length}})</view>
			<view @click="isShow">{{is_show?'管理':'完成'}}</view>
		</view>
		
		
		<view class="pb-5 mb-3">
			<view class="p-3 bg-white mx-3 mb-2 radius10 d-flex flex-column a-end"
			v-for="(item,index) in mainData" :key="index">
				<view class="d-flex a-center j-sb mb-2">
					<image src="../../static/images/shopping-icon.png" class="shop-icon"></image>
					<!-- <image src="../../static/images/shopping-icon1.png" class="shop-icon"></image> -->
					<image :src="item.mainImg[0].url" class="shopImg mx-2"></image>
					<view class="shopCon d-flex flex-column flex-1">
						<view class="tit avoidOverflow pb-2">{{item.title}}</view>
						<view class="font-22 color6 line-h flex-1">规格：{{item.sku.title}}</view>
						<view class="colorR font-32">
							<text class="price1 font-w pb-2">{{item.sku.o_price}}</text>/
							<text class="priceV font-w">{{item.sku.price}}</text>
						</view>
					</view>
				</view>
				<view class="d-flex a-center count">
					<image src="../../static/images/shopping-icon2.png" class="count-icon1" @click="count(-1,index)"></image>
					<view class="num text-center f5bj">{{item.num}}</view>
					<image src="../../static/images/shopping-icon3.png" class="count-icon2" @click="count(1,index)"></image>
				</view>
			</view>
		</view>
		
		
		<view style="height: 140rpx;"></view>
		<view class="bg-white p-f left-0 right-0 d-flex p-f carBot" v-if="is_show">
			<view class="font-26 d-flex a-center j-sb px-3 flex-1">
				<view class="d-flex a-center">
					<image src="../../static/images/shopping-icon1.png" class="shop-icon"></image>
					<view class="pl-1">全选</view>
				</view>
				<view class="font-22 d-flex a-center">合计 <text class="price font-30 font-w">{{totle}}</text></view>
			</view>
			<view class="carBtn" @click="Router.navigateTo({route:{path:'/pages/shop-order/shop-order'}})">立即购买</view>
		</view>
		<view class="bg-white p-f left-0 right-0 d-flex a-center j-sb px-3 carBot" v-else>
			<view class="d-flex a-center">
				<image src="../../static/images/shopping-icon1.png" class="shop-icon"></image>
				<view class="pl-1 font-26">全选</view>
			</view>
			<view class="font-23 borderM radius30 deleteBtn">删除</view>
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
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/shop-car/shop-car'}})">
				<image src="../../static/images/nabar3-a.png" mode=""></image>
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
				is_show:true,
				mainData:[],
				totle:0
			}
		},
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('carData');
			self.totlePrice()
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			count(i,index){
				const self = this;
				if(self.mainData[index].num+i > 0){
					self.mainData[index].num += i;
					self.totlePrice()
				}
			},
			
			totlePrice(){
				const self = this;
				if(self.mainData){
					self.totle = 0;
					for(var i=0;i<self.mainData.length;i++){
						var totle = self.mainData[i].num * parseFloat(self.mainData[i].sku.price)
						self.totle = totle+self.totle
						console.log(self.totle)
					}
				}
				// self.totle = self.totle.toFixed(2);
			}
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
