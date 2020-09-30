<template>
	<view>
		
		<!-- banner -->
		<view class="banner">
			<swiper class="swiper-box" style="height: 400rpx;" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#FF6F48">
				<block v-for="(item,index) in sliderData" :key="index">
					<swiper-item class="swiper-item" :data-id = "item.passage1"
			 @click="item.passage1!=''?Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail?id='+$event.currentTarget.dataset.id}}):''">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view  v-if="noteData.mainImg&&noteData.mainImg[0]" class="mt-2 mb-2">
			<image style="width: 100%;" mode="widthFix" :src="noteData.mainImg&&noteData.mainImg[0]?noteData.mainImg[0].url:''"> 
		</view>
		<!-- 金刚区 -->
		<view class="flex mx-3 font-24 bg-white mt-2 radius10 pb-4 flex-wrap">
			<view class="tt flex4 pt-4" v-for="(item,index) in labelData" :key="index" 
			 @click="Router.navigateTo({route:{path:'/pages/shop-classify/shop-classify?id='+index}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"  ></image>
				<view class="font-28">{{item.title}}</view>
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
				v-show="index<3" 
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">{{item.title}}</view>
					<view class="price font-w pb-2">{{item.sku&&item.sku[0]?item.sku[0].o_price:''}}</view>
					<view class="priceV font-w font-26"  style="color: #000000;">{{item.sku&&item.sku[0]?item.sku[0].price:''}}</view>
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
			<view class="shopHot font-32 line-h flex">
				<view class="pb-3 mr-2 hotGood" 
				v-for="(item,index) in recommendData" :key="index" 
				v-show="index<3"  
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">{{item.title}}</view>
					<view class="price font-w pb-2">{{item.sku&&item.sku[0]?item.sku[0].o_price:''}}</view>
					<view class="priceV font-w font-26"  style="color: #000000;">{{item.sku&&item.sku[0]?item.sku[0].price:''}}</view>
				</view>
			</view>
			<!-- <view class="shopJp font-32 line-h flex1">
				<view class="pb-3" 
				v-for="(item,index) in recommendData" :key="index" 
				v-show="index<3"  @click="goDetail(item)">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					<view class="tit font-26 pt-2 pb-3 avoidOverflow">{{item.title}}</view>
					<view class=" font-24">
						<text class="price1 font-w pb-2">{{item.sku&&item.sku[0]?item.sku[0].o_price:''}}</text>/
						<text class="priceV font-w">{{item.sku&&item.sku[0]?item.sku[0].price:''}}</text>
					</view>
				</view>
			</view> -->
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
			v-for="(item,index) in mainData" :key="index"
			:data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				<view class="bg-white p-2">
					<view class="tit avoidOverflow2 font-26">{{item.title}}</view>
					<view class="font-20 color9 line-h py-2">销量 {{item.sale_count}}</view>
					<view class="">
						<text class="price1 font-w pb-2 font-30">{{item.sku&&item.sku[0]?item.sku[0].o_price:''}}</text>
						<text class="font-w font-24 priceV"  style="color: #000000;margin-left: 10rpx;">¥{{item.sku&&item.sku[0]?item.sku[0].price:''}}</text>
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
			<view class="item" @click="toPath('/pages/shop-car/shop-car')">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>购物车</view>
			</view>
			<view class="item" @click="toPath('/pages/shop-user/shop-user')">
				<image src="../../static/images/nabar4.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
		<view class="bg-mask" v-show="is_show1">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">
					<view class="content ql-editor text-center" style="padding:0;" v-html="tipsData.content">
					</view>
				</view>
				<view class="" style="margin-bottom: 30rpx;color: #888;">联系客服：{{kefuPhone}}</view>
				<view class="flex1 bT-f5 btn1">
					<view class="w-50 bR-f5" @click="showToast1">我知道了</view>
					<view class="colorM w-50" @click="phoneCall()">联系客服</view>
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
				labelData:[],
				mainData:[],
				recommendData:[],
				hotData:[],
				searchItem:{
					thirdapp_id: 2,
					type: 1
				},
				isLoadAll:false,
				sliderData:{},
				is_show1: false,
				tipsData: {},
				kefuData: {},
				noteData:{},
				kefuPhone:''
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/shop-index/shop-index')
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData','getNoteData','getMainData','getSliderData','getTipsData','getKefuData'], self);
		},
		
		onShow() {
			const self = this;
			self.getUserInfoData();
		},
		
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getNoteData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id:97
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.noteData = res.info.data[0]
					}
					self.$Utils.finishFunc('getNoteData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getKefuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id: 3,
					thirdapp_id: 2
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.kefuData = res.info.data[0];
					}
					self.$Utils.finishFunc('getKefuData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.getAfter = {
					staff:{
						tableName:'User',
						middleKey:'staff_no',
						key:'staff_no',
						searchItem:{
							status:1,
							user_type:1
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0].info;
						if (self.userInfoData.behavior == 1 || self.userInfoData.user_type == 1) {
							self.isVip = true
						};
						if(res.info.data[0].staff&&res.info.data[0].staff[0]&&res.info.data[0].staff[0].info&&res.info.data[0].staff[0].info.phone!=''){
							self.kefuPhone = res.info.data[0].staff[0].info.phone
						}else{
							self.kefuPhone = uni.getStorageSync('user_info').thirdApp.phone
						}
					}
				}
				self.$apis.userGet(postData, callback);
			},
			
			phoneCall() {
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.kefuPhone
				})
			},
			
			showToast1() {
				const self = this;
				self.is_show1 = false
			},
			
			getTipsData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '普通会员提示（商城）',
					menu_id: 10
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.tipsData = res.info.data[0]
					}
					console.log('menu', self.tipsData)
					self.$Utils.finishFunc('getTipsData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			toPath(path){
				const self = this;
				if(!self.isVip){
					self.is_show1 = true;
					return
				};
				self.Router.redirectTo({route:{path:path}})
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id:49
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.sliderData = res.info.data
					}
					self.$Utils.finishFunc('getSliderData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
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
.banner image{height: 400rpx;}
.tt{width: 33.3%;}
.tt image{width: 100rpx;height: 100rpx;margin-bottom: 20rpx}

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

.nav .on {
		color: #222;
	}

	.hot-mask {
		width: 100%;
		height: 100%;
		background-color: #F5F5F5;
		opacity: 0.6;
		position: absolute;
		z-index: 100;
	}

	.tc {
		width: 620rpx;
		margin: auto;
		left: 0;
		right: 0;
		top: 30%;
	}

	.tc .txt {
		padding: 60rpx;
	}

	.tc .btn1 {
		line-height: 100rpx;
	}

	.mhBox {
		filter: blur(1px);
	}

	.mh {
		filter: blur(6px);
	}
</style>
