<template>
	<view>
		
		<!-- banner -->
		<view class="banner radius20 overflow-h mx-3 mt-2"  @click="Router.redirectTo({route:{path:'/pages/shop-classify/shop-classify'}})">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#FF6F48">
				<block>
					<swiper-item class="swiper-item">
						<image src="../../static/images/mall-banner.png" class="slide-image" />
					</swiper-item>
				</block>
				<block>
					<swiper-item class="swiper-item">
						<image src="../../static/images/mall-banner.png" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<!-- 金刚区 -->
		<view class="flex1 mx-3 font-24 bg-white mt-2 radius10 pb-4">
			<view class="tt flex4 pt-4" v-for="(item,index) in labelData" :key="index" v-show="index>0"
			 @click="Router.navigateTo({route:{path:'/pages/shop-classify/shop-classify?id='+index}})">
				<image :src="item.mainImg[0].url" ></image>
				<view>{{item.title}}</view>
			</view>
		</view>
		
		<!-- 热门活动 -->
		<view class="mx-3 font-26 bg-white mt-2 radius10 p-3 pb-0">
			<view class="flex1 pb-3"> 
				<view class="flex font-34 font-w">
					<image src="../../static/images/mall-icon.png" class="mall-icon"></image>
					<view>热门活动</view>
				</view>
				<view class="flex font-24 color9"
				@click="Router.navigateTo({route:{path:'/pages/shop-classifyGood/shop-classifyGood?type=hot'}})">
					<view>更多</view>
					<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
				</view>
			</view>
			<view class="shopHot font-32 line-h flex">
				<view class="pb-3 mr-2 hotGood" 
				v-for="(item,index) in hotData" :key="index" 
				v-show="index<3"  @click="goDetail(item)">
					<image :src="item.mainImg[0].url" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">{{item.title}}</view>
					<view class="price1 font-w pb-2">{{item.sku[0].o_price}}</view>
					<view class="priceV font-w">{{item.sku[0].price}}</view>
				</view>
			</view>
		</view>
		
		<!-- 精品推荐 -->
		<view class="mx-3 font-26 bg-white mt-2 radius10 p-3 pb-0">
			<view class="flex1 pb-3"> 
				<view class="flex font-34 font-w">
					<image src="../../static/images/mall-icon1.png" class="mall-icon"></image>
					<view>精品推荐</view>
				</view>
				<view class="flex font-24 color9"
				@click="Router.navigateTo({route:{path:'/pages/shop-classifyGood/shop-classifyGood?type=recommend'}})">
					<view>更多</view>
					<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
				</view>
			</view>
			<view class="shopJp font-32 line-h flex1">
				<view class="pb-3" 
				v-for="(item,index) in recommendData" :key="index" 
				v-show="index<2"  @click="goDetail(item)">
					<image :src="item.mainImg[0].url" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">{{item.title}}</view>
					<view class="colorR">
						<text class="price1 font-w pb-2">{{item.sku[0].o_price}}</text>/
						<text class="priceV font-w">{{item.sku[0].price}}</text>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 为你推荐 -->
		<view class="flex0 pt-4">
			<view class="line100"></view>
			<view class="flex font-34 font-w px-3">
				<image src="../../static/images/mall-icon2.png" class="mall-icon"></image>
				<view>为你推荐</view>
			</view>
			<view class="line100"></view>
		</view>
		<view class="flex1 px-3">
			<view class="radius10 overflow-h pt-3 shopTj" 
			v-for="(item,index) in mainData" :key="index" @click="goDetail(item)">
				<image :src="item.mainImg[0].url" mode=""></image>
				<view class="bg-white p-2">
					<view class="tit avoidOverflow2 font-26">{{item.title}}</view>
					<view class="font-20 color9 line-h py-2">销量 {{item.sale_count}}</view>
					<view class="colorR font-32">
						<text class="price1 font-w pb-2">{{item.sku[0].o_price}}</text>/
						<text class="priceV font-w">{{item.sku[0].price}}</text>
					</view>
				</view>
			</view>
		</view>
		
		
		
		<view style="height: 140rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
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
				labelData:[],
				mainData:[],
				recommendData:[],
				hotData:[],
				searchItem:{
					thirdapp_id: 2,
					type: 1
				},
				isLoadAll:false
			}
		},
		onLoad(){
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData','getMainData'], self);
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			goDetail(item){
				const self = this;
				uni.setStorageSync('productDetail',item)
				self.Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail'}})
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					type: 3
				}
				postData.order = {
					listorder:'asc'
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData = res.info.data;
					}
					console.log('label',self.labelData);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.paginate);
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						for(var i=0;i<res.info.data.length;i++){
							if(res.info.data[i].recommend==1){
								self.recommendData.push(res.info.data[i])
							}
							if(res.info.data[i].hot == 1){
								self.hotData.push(res.info.data[i])
							}
							self.mainData.push(res.info.data[i])
						}
						console.log('main',self.mainData);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.productGet(postData, callback);
			}
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.banner image{height: 280rpx;border-radius: 10rpx;}
.tt{width: 25%;}
.tt image{width: 90rpx;height: 90rpx;margin-bottom: 20rpx}

.mall-icon{width: 44rpx;height: 43rpx;margin-right: 10rpx;}
.shopHot image{width: 190rpx;height: 190rpx;border-radius: 10rpx;}
.shopHot .tit{width: 190rpx;}
.hotGood:nth-child(3){margin: 0;}
.shopJp image{width: 300rpx;height: 200rpx;border-radius: 10rpx 10rpx 0 0;}
.shopJp .tit{width: 300rpx;}
.line100{width: 100rpx;border-top: 1px solid #e1e1e1;}
.shopTj{width: 330rpx;}
.shopTj image{width: 100%;height: 300rpx;border-radius: 10rpx 10rpx 0 0;}
.shopTj .tit{width: 290rpx;height: 70rpx;}
</style>
