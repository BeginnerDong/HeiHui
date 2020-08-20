<template>
	<view class="line-h">
		
		<!-- banner -->
		<view class="banner p-r">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#FF6F48">
				<block class="swiper-item" v-for="(item,index) in mainData.bannerImg">
					<swiper-item>
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<view class="bg-white p-3 mb-2">
			<view class="font-32 pb-4 font-w">{{mainData.title}}</view>
			<view class="flex1">
				<view class="colorR font-32">
					<text class="price1 font-w pb-2">{{mainData.sku&&mainData.sku[0]&&mainData.sku[0].o_price}}</text>/
					<text class="priceV font-w">{{mainData.sku&&mainData.sku[0]&&mainData.sku[0].price}}</text>
				</view>
				<view class="font-22 color9">销量 {{mainData.sale_count}}</view>
			</view>
		</view>
		
		<view class="px-3 py-4 flex1 bg-white mb-2">
			<view class="font-w">规格</view>
			<view class="font-24 flex-1 d-flex j-sb pl-3" @click="ggShow">
				<view class="flex-1 color9">请选择</view>
				<image src="../../static/images/goods details-icon.png" class="gg-icon"></image>
			</view>
		</view>
		
		<view class="py-4 bg-white mb-2">
			<view class="px-3 pb-3 font-w">详情描述</view>
			<view>
				<!-- <image src="../../static/images/goods details-img2.png" class="detailImg"></image> -->
				<view class="content ql-editor px-3" v-html="mainData.content">
					
				</view>
			</view>
		</view>
		
		<view style="height: 100rpx;"></view>
		<view class="bg-white text-center p-f bottom-0 left-0 right-0 flex1 bT-f5 px-3">
			<view class="" @click="Router.navigateTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/goods details-icon1.png" class="fh-icon"></image>
				<view class="font-20 pt-1">返回</view>
			</view>
			<view class="" @click="Router.navigateTo({route:{path:'/pages/shop-car/shop-car'}})">
				<image src="../../static/images/goods details-icon2.png" class="car-icon"></image>
				<view class="font-20 pt-1">购物车</view>
			</view>
			<view class="flex1 btnBox font-w colorf">
				<view class="btn" @click="ggShow">加入购物车</view>
				<view class="btn" @click="ggShow">立即购买</view>
			</view>
		</view>
		
		
		<!-- 规格 -->
		<view class="bg-mask line-h" v-show="gg_show">
			<view class="radius20-T bg-white p-a bottom-0 left-0 right-0 pt-3 d-flex flex-column ggBox" >
				<view class="flex a-end pb-3 px-3">
					<view class="gg"><image :src="skuData[skuCurr]&&skuData[skuCurr].mainImg&&skuData[skuCurr].mainImg[0]&&skuData[skuCurr].mainImg[0].url"></image></view>
					<view class="pl-2 flex-1">
						<view class="font-32 pb-3 colorR">
							<text class="price1 font-w pb-2">{{skuData[skuCurr]&&skuData[skuCurr].o_price}}</text>/
							<text class="priceV font-w">{{skuData[skuCurr]&&skuData[skuCurr].price}}</text>
						</view>
						<view class="font-24 color9">{{skuData[skuCurr]&&skuData[skuCurr].title}}</view>
					</view>
					<view @click="ggShow">
						<image src="../../static/images/goods details-icon3.png" class="x-icon"></image>
					</view>
				</view>
				
				<view class="flex-1 flexY flex-column mb-3 px-3 ggPartBox">
					<view class="ggPart pb-1">
						<view class="py-3 font-w">规格</view>
						<view class="font-24 color6">
							<view class="span" v-for="(item,index) in mainData.sku" :key="index"
							:class="skuCurr==index?'on':''" @click="changeSku(index)">{{item.title}}</view>
						</view>
					</view>
					<view class="ggPart pb-1 flex1">
						<view class="py-3 font-w">购买数量</view>
						<view class="d-flex a-center count">
							<image src="../../static/images/shopping-icon2.png" class="count-icon1" @click="count(-1)"></image>
							<view class="num text-center f5bj">{{num}}</view>
							<image src="../../static/images/shopping-icon3.png" class="count-icon2" @click="count(1)"></image>
						</view>
					</view>
				</view>
				
				<view class="font-30 colorf text-center flex1 bT-e1 px-3 p-a left-0 right-0 bottom-0">
					<view class="ggBtn" @click="addCar()">加入购物车</view>
					<view class="ggBtn" @click="goBuy()">立即购买</view>
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
				gg_show:false,
				mainData:{},
				skuData:{},
				skuCurr:0,
				num:1
			}
		},
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('productDetail');
			self.skuData = self.$Utils.cloneForm(self.mainData.sku);
			console.log('main',self.mainData)
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		methods: {
			
			goBuy(){
				const self = this;
				if(!self.mainData.sku[self.skuCurr]){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('商品暂无可选规格！', 'none');
					return
				};
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{sku_id:self.mainData.sku[self.skuCurr].id,count:1,
					type:self.mainData.type,product:self.mainData,skuIndex:self.skuCurr},
				);
				uni.setStorageSync('payPro', self.orderList);
				self.gg_show = !self.gg_show
				self.Router.navigateTo({route:{path:'/pages/shop-order/shop-order'}})
				uni.setStorageSync('canClick',true);
			},
			
			addCar() {
				const self = this;
				var obj = self.mainData;
				self.mainData.skuIndex = self.skuCurr;
				self.mainData.skuId = self.mainData.sku[self.skuCurr].id;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if (array[i].skuId == self.mainData.sku[self.skuCurr].id) {
						var target = array[i]
					}
				}
				
				console.log(target)
				if (target) {
					target.count = target.count + 1
				} else {
					console.log(111)
					var target = self.mainData;
					target.count = 1;
					target.isSelect = true;
				}
				self.$Utils.showToast('加入成功', 'none');
				self.$Utils.setStorageArray('cartData', target, 'skuId', 999);
			},
			
			ggShow(){
				const self = this;
				self.gg_show = !self.gg_show
			},
			
			changeSku(index){
				const self = this;
				self.skuCurr = index;
			},
			
			count(i){
				const self = this;
				if(self.num+i > 0){
					self.num += i;
				}
			},
			
			/* addCar(sku){
				const self = this;
				var data = self.$Utils.cloneForm(self.mainData);
				data.sku = sku;
				data.num = self.num;
				var carData = [];
				if(uni.getStorageSync('carData')){
					carData = uni.getStorageSync('carData');
					for(var i=0;i<carData.length;i++){
						if(data.id == carData[i].id && data.sku.id == carData[i].sku.id){
							self.$Utils.showToast('商品已存在购物车','none')
							return
						}else{
							carData.push(data)
							uni.setStorageSync('carData',carData)
						}
					}
				}else{
					carData.push(data)
					uni.setStorageSync('carData',carData)
					self.$Utils.showToast('已加入购物车','none')
				}
				self.gg_show = !self.gg_show
			}, */
			
			/* goBuy(sku){
				const self = this;
				var data = self.$Utils.cloneForm(self.mainData);
				data.sku = sku;
				data.num = self.num;
				var buyOrder = data;
				uni.setStorageSync('buyOrder',buyOrder)
				self.Router.navigateTo({route:{path:'/pages/shop-order/shop-order'}})
			} */
			
		}
	}
</script>
<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.banner .swiper-box{width: 750rpx;height: 750rpx;}
.banner image{width: 750rpx;height: 750rpx;}

.gg-icon{width: 38rpx;height: 10rpx;}
.detailImg{width: 750rpx;height: 3950rpx;}

.fh-icon{width: 41rpx;height: 33rpx;margin: auto;}
.car-icon{width: 38rpx;height: 38rpx;margin: auto;}
.btnBox{width: 460rpx;}
.btn{width: 220rpx;line-height: 80rpx;background-color: #6F7178;margin: 20rpx 0;border-radius: 40rpx;}
.btn:nth-child(2){background-color: #F59824;}

.ggBox{min-height: 900rpx;}
.gg{width: 182rpx;}
.gg image{width: 182rpx;height: 182rpx;border-radius: 10rpx;}
.span{background-color: #f5f5f5;padding: 0 20rpx;margin-right: 30rpx;margin-bottom: 20rpx;display: inline-block;line-height: 56rpx;border-radius: 28rpx;}
.ggPart .on{background-color: #FFF7ED;color: #E39423;border: 1px solid #E39423;}
.ggBtn{line-height: 80rpx;background-color: #6F7178;width: 330rpx;border-radius: 40rpx;margin: 20rpx 0;}
.ggBtn:nth-child(2){background-color: #F59824;}
.ggPartBox{max-height: 600rpx;}
</style>
