<template>
	<view>
		
		<view class="d-flex j-sb p-3 bB-f5">
			<view>全部商品({{mainData.length?mainData.length:'0'}})</view>
			<view @click="allDeltShow">{{!is_allDelt?'管理':'完成'}}</view>
		</view>
		
		
		<view class="pb-5 mb-3">
			<view class="p-3 bg-white mx-3 mb-2 radius10 d-flex flex-column a-end"
			v-for="(item,index) in mainData" :key="index">
				<view class="d-flex a-center j-sb mb-2">
					<image  :src="item.isSelect?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" @click="choose(index)" class="shop-icon"></image>
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="shopImg mx-2"></image>
					<view class="shopCon d-flex flex-column flex-1">
						<view class="tit avoidOverflow pb-2">{{item.title}}</view>
						<view class="font-22 color6 line-h flex-1">规格：{{item.sku&&item.sku[item.skuIndex]?item.sku[item.skuIndex].title:''}}</view>
						<view class="colorR ">
							<text class="price1 font-w pb-2 font-32">{{item.sku&&item.sku[item.skuIndex]?item.sku[item.skuIndex].o_price:''}}</text>
							<text class="priceV font-w font-28" style="color: #000000;margin-left: 10rpx;">{{item.sku&&item.sku[item.skuIndex]?item.sku[item.skuIndex].price:''}}</text>
						</view>
					</view>
				</view>
				<view class="d-flex a-center count">
					<image src="../../static/images/shopping-icon2.png" class="count-icon1" @click="counter(index,'-')"></image>
					<view class="num text-center f5bj">{{item.count}}</view>
					<image src="../../static/images/shopping-icon3.png" class="count-icon2" @click="counter(index,'+')"></image>
				</view>
			</view>
		</view>
		
		
		<view style="height: 140rpx;"></view>
		<view class="bg-white p-f left-0 right-0 d-flex p-f carBot" v-if="!is_allDelt">
			<view class="font-26 d-flex a-center j-sb px-3 flex-1">
				<view class="d-flex a-center">
					<image  @click="chooseAll" :src="isChooseAll?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" class="shop-icon"></image>
					<view class="pl-1">全选</view>
				</view>
				<view class="font-22 d-flex a-center">合计 <text class="price font-30 font-w">{{totalPrice}}</text></view>
			</view>
			<view class="carBtn" @click="pay">立即购买</view>
		</view>
		<view class="bg-white p-f left-0 right-0 d-flex a-center j-sb px-3 carBot" v-else>
			<view class="d-flex a-center">
				<image @click="chooseAll" :src="isChooseAll?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" class="shop-icon"></image>
				<view class="pl-1 font-26">全选</view>
			</view>
			<view class="font-23 borderM radius30 deleteBtn" @click="deleteAll()">删除</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
				count:1,
				mainData:[
					
				],
				is_allDelt:false,
				totalPrice:0,
				isChooseAll:true
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.mainData = self.$Utils.getStorageArray('cartData');
			console.log('self.mainData',self.mainData)
			self.checkChooseAll();
			self.countTotalPrice();
		},
		
		methods: {
			
			pay(e) {
				const self = this;
				const orderList = [
				];
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						orderList.push(
							{sku_id:self.mainData[i].sku[self.mainData[i].skuIndex].id,count:self.mainData[i].count,
							product:self.mainData[i],skuIndex:self.mainData[i].skuIndex},
						);
					};
				};
				if (orderList.length == 0) {
					self.$Utils.showToast('未选择商品', 'none', 1000);
					return;
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/shop-order/shop-order'
					}
				})
			},
			
			checkChooseAll() {
				const self = this;
				var isChooseAll = true;
				for (var i = 0; i < self.mainData.length; i++) {
					if (!self.mainData[i].isSelect) {
						isChooseAll = false;
					};
				};
				self.isChooseAll = isChooseAll;
			},
			
			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].isSelect = self.isChooseAll;
					self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
				};
				self.countTotalPrice();
			},
			
			allDeltShow(){
				const self = this;
				self.is_allDelt = !self.is_allDelt
			},
			
			counter(index,type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				self.countTotalPrice();
			},
			
			deleteAll() {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认要删除选中商品吗？',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							for (var i = 0; i < self.mainData.length; i++) {
								if(self.mainData[i].isSelect){
									self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
								}
							};
							self.mainData = self.$Utils.getStorageArray('cartData');
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					},
				});
			},
			
			
			
			choose(index) {
				const self = this;
				
				if (self.mainData[index].isSelect) {
					self.mainData[index].isSelect = false;
				} else {
					self.mainData[index].isSelect = true;
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.checkChooseAll();
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						self.totalPrice += self.mainData[i].sku[self.mainData[i].skuIndex].price * self.mainData[i].count;
					};
				};
				self.totalPrice = self.totalPrice.toFixed(2)
				console.log(self.totalPrice)
			},
			
			
			
		}
	};
</script>




<style>
page{background-color: #f5f5f5;}
</style>
