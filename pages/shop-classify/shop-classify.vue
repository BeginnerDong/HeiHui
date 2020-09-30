<template>
	<view class="h-100">
		
		<view class="h-100 p-fXY">
			<view class="left line-h h-100 font-26 color6 p-a bg-f5">
				<view :data-id="'item'+index" v-for="(item,index) in labelData" @tap="bindToView" :data-index="index" 
				:class="toView=='item'+index?'on':''" 
				class="py-3 classLi">{{item.title}}</view>
			</view>
			
			<scroll-view class="radius10 bg-white mr-3 mt-3 font-26 right" :scroll-into-view="toView" scroll-y="true" scroll-with-animation="true" @scroll="view" >
				<view class="px-3" :id="'item'+index"  :class="'item'+index">
					<view class="font-30 font-w pt-2 pb-3"></view>
					<view class="d-flex a-start flex-wrap">
						<view class="flex4 pb-3 tjItem"
						@click="Router.navigateTo({route:{path:'/pages/shop-classifyGood/shop-classifyGood?id='+c_item.id}})"
						v-for="(c_item,c_index) in current.children" :key="c_index">
							<image :src="c_item.mainImg&&c_item.mainImg[0]?c_item.mainImg[0].url:''" class="classImg mb-2"></image>
							<view class="text-center">{{c_item.title}}</view>
						</view>
					</view>
				</view>
			</scroll-view>
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
				leftCurr:0,
				title:'item0',
				labelData:[],
				list:['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z'],
				toView:'item0',
				is_show1: false,
				tipsData: {},
				kefuData: {},
				current:{},
				kefuPhone:''
			}
		},
		onLoad(option){
			const self = this;
			var options = self.$Utils.getHashParameters();
			console.log(options);
			if(options[0].id){
				self.toView = 'item'+options[0].id;
				self.isHasOption = true;
				self.isIndex = options[0].id
			}
			
			
			self.$Utils.loadAll(['getLabelData','getTipsData','getKefuData'], self);
		},
		onShow() {
			const self = this;
			self.getUserInfoData();
		},
		
		methods: {
			
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
					//menu_id: 10
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
			
			bindToView(e){
				const self = this;
				var id = e.currentTarget.dataset.id;
				var index = e.currentTarget.dataset.index;
				this.toView = id;
				self.current = self.labelData[index]
			},
			
			view(e){
				const query = uni.createSelectorQuery().in(this);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.order = {
					listorder:'asc'
				}
				postData.searchItem = {
					type: 3
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData =  res.info.data;
						if(self.isHasOption){
							self.current =self.labelData[self.isIndex]
							console.log(232443)
						}else{
							self.current = self.labelData[0]
						}
						
					}
					console.log('label',self.labelData);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
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
.right{overflow-y: auto;box-sizing: border-box;margin-bottom: 140rpx;margin-left: 180rpx;height: 89%;}
uni-scroll-view{width: auto!important;}

.tjItem{width: 33.33%;}

.classImg{width: 120rpx;height: 120rpx;}
.classItem{width: 33.33%;}

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
