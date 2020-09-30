<template>
	<view class="line-h">

		<!-- banner -->
		<view class="banner p-r">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#FF6F48">
				<block class="swiper-item" v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item :data-url="item.url" @click="getUrl($event.currentTarget.dataset.url)">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>

		<view class="bg-white p-3 mb-2">
			<view class="font-32 pb-2 font-w">{{mainData.title}}</view>
			<view class="font-28 pb-2 color9">{{mainData.description}}</view>
			<view class="flex1">
				<view class="colorR">
					<text class="price1 font-w pb-2 font-30">{{mainData.sku&&mainData.sku[specsCurr]?mainData.sku[specsCurr].o_price:''}}</text>
					<text class="priceV font-w font-24"  style="color: #000000;margin-left: 20rpx;">{{mainData.sku&&mainData.sku[specsCurr]?mainData.sku[specsCurr].price:''}}</text>
				</view>
				<view class="font-22 color9">销量 {{mainData.sale_count}}</view>
			</view>
		</view>

		<view class="px-3 py-4 flex1 bg-white mb-2" v-if="mainData.sellout==0">
			<view class="font-w">规格</view>
			<view class="font-24 flex-1 d-flex j-sb pl-3" @click="ggShow">
				<view class="flex-1 color9">请选择</view>
				<image src="../../static/images/goods details-icon.png" class="gg-icon"></image>
			</view>
		</view>
		<view class="px-3 py-4 flex1 bg-white mb-2">
			<view class="font-w">运费</view>
			<view class="font-24 flex-1 d-flex j-sb pl-3">
				<view class="flex-1 color9">{{mainData.transport_fee}}</view>
			</view>
		</view>
		<view class="py-4 bg-white mb-2">
			<view class="px-3 pb-3 font-w">详情描述</view>
			<view>
				<!-- <image src="../../static/images/goods details-img2.png" class="detailImg"></image> -->
				<view class="content ql-editor px-3" @click="getImg($event)" v-html="mainData.content">

				</view>
			</view>
		</view>

		<view style="height: 100rpx;"></view>
		<view class="bg-white text-center p-f bottom-0 left-0 right-0 flex1 bT-f5 px-3">
			<view class="" @click="Router.navigateTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/goods details-icon1.png" class="fh-icon"></image>
				<view class="font-20 pt-1">返回</view>
			</view>
			<view class="" @click="toCar">
				<image src="../../static/images/goods details-icon2.png" class="car-icon"></image>
				<view class="font-20 pt-1">购物车</view>
			</view>
			<view class="flex1 btnBox font-w colorf" v-if="mainData.sellout==0">
				<view class="btn" @click="ggShow">加入购物车</view>
				<view class="btn" @click="ggShow">立即购买</view>
			</view>
			
			<view class="flex1 btnBox font-w colorf" v-if="mainData.sellout==1">
				<view class="btn" style="width: 100%;">商品已售罄</view>
			</view>
		</view>


		<!-- 规格 -->
		<view class="bg-mask line-h" v-show="gg_show">
			<view class="radius20-T bg-white p-a bottom-0 left-0 right-0 pt-3 d-flex flex-column ggBox">
				<view class="flex a-end pb-3 px-3">
					<view class="gg">
						<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''"></image>
					</view>
					<view class="pl-2 flex-1">
						<view class=" pb-3 colorR">
							<text class="price1 font-w pb-2 font-32">{{mainData.sku[specsCurr]&&mainData.sku[specsCurr].o_price}}</text>
							<text class="priceV font-w font-26" style="color: #000000;margin-left: 10rpx;">{{mainData.sku[specsCurr]&&mainData.sku[specsCurr].price}}</text>
						</view>
						<view class="font-26 color2">
							<span v-for="(item,index) in labelData" :key="index">
								{{item.title}}:
								<span v-for="(c_item,c_index) in item.children" :key="c_item.id" 
									v-show = "Utils.inArray(c_item.id,sku_item)!=-1"
									class="mr-2"
								>{{c_item.title}}</span>
							</span>|<span class="ml-2">数量：{{num}}</span>
						</view>
					</view>
					<view @click="ggShow">
						<image src="../../static/images/goods details-icon3.png" class="x-icon"></image>
					</view>
				</view>

				<view class="flex-1 flexY flex-column mb-3 px-3 ggPartBox">
					<view class="ggPart pb-1" v-for="(item,index) in labelData" :key="index">
						
						<view class="py-3 font-w">{{item.title}}</view>
						<view class="font-24 color6">
							<view class="span" 
								v-for="(c_item,c_index) in item.children" :key="c_item.id"
								:class="Utils.inArray(c_item.id,choose_sku_item)==-1?'cantChoose'
								:(Utils.inArray(c_item.id,sku_item)!=-1?'on':'')"
								:data-id = "item.id" :data-c_id="c_item.id"
								@click="Utils.inArray($event.currentTarget.dataset.c_id,choose_sku_item)!=-1?
								chooseSku($event.currentTarget.dataset.id,$event.currentTarget.dataset.c_id):''"
							>
								{{c_item.title}}
							</view>
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

		<view class="bg-mask" v-show="is_show1">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">
					<view class="content ql-editor text-center"   style="padding:0;" v-html="tipsData.content">
					</view>
				</view>
				<view class="" style="margin-bottom: 30rpx;color: #888;">联系客服：{{kefuPhone}}</view>
				<view class="flex1 bT-f5 btn1">
					<view class="w-50 bR-f5" @click="showToast1">我知道了</view>
					<view class="colorM w-50" @click="phoneCall()">联系客服</view>
				</view>
			</view>
		</view>
		
		<template>
		    <view>
		        <previewImage ref="previewImage" :opacity="1" :imgs="param"></previewImage>
		    </view>
		</template>
	</view>
</template>

<script>
	import previewImage from '@/components/kxj-previewImage/kxj-previewImage.vue';
	export default {
		components: { previewImage}, //注册插件
		data() {
			return {
				Router: this.$Router,
				gg_show: false,
				mainData: {},
				skuData: [],
				specsCurr: 0,
				num: 1,
				is_show1: false,
				tipsData: {},
				kefuData: {},
				param:[],
				Utils:this.$Utils,
				labelData:[],
			
				
				sku_item:[],
				choose_sku_item:[],
				kefuPhone:''
			}
		},
		onLoad(options) {
			const self = this;
			var options = self.$Utils.getHashParameters();
			console.log(options);
			self.id = options[0].id;
			self.$Utils.loadAll(['wxJsSdk','getMainData','getTipsData','getKefuData'], self);
			window.getImg = function($event){
				console.log($event);
				self.$jweixin.previewImage({
					current:0,
					urls:[$event.target.currentSrc]
				})
			};
		},

		onShow() {
			const self = this;
			self.getUserInfoData();
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},

		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'category_id',
						key:'category_id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img onclick='self.getImg(event)'"`);
						console.log('self.mainData',self.mainData)
						for(var key in self.mainData.label){
						  if(self.mainData.sku_array.indexOf(parseInt(key))!=-1){
						    self.labelData.push(self.mainData.label[key])
						  };    
						};
						console.log('self.labelData',self.labelData)
						for (var i = 0; i < self.mainData.sku.length; i++) {
						  /* if(self.mainData.sku[i].id==self.id){
						    self.skuData = self.$Utils.cloneForm(self.mainData.sku[i]);
						  }; */
										
						  self.choose_sku_item.push.apply(self.choose_sku_item,self.mainData.sku[i].sku_item);
						};
						self.skuData = self.$Utils.cloneForm(self.mainData.sku[0]);
						self.sku_item = self.skuData.sku_item;
						console.log('main', self.mainData)
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.productGet(postData, callback);
			},
			
			chooseSku(parentid,id){
				const self = this;
				console.log(23232)
			    self.skuData = {};
			    if(self.choose_sku_item.indexOf(parseInt(id))==-1){
					console.log('self.choose_sku_item',self.choose_sku_item.indexOf(id))
					console.log('self.choose_sku_item',self.choose_sku_item)
					console.log('id',id)
					console.log('parentid',parentid)
			      return;
			    };
			    self.choose_sku_item = [];
				console.log('parentid',parentid)
			    var sku = self.mainData.label[parentid];
				console.log('sku',sku)
			    for(var i=0;i<sku.children.length;i++){
			      if(self.sku_item.indexOf(sku.children[i].id)!=-1){
			        self.sku_item.splice(self.sku_item.indexOf(sku.children[i].id), 1);
			      };
			      self.choose_sku_item.push(sku.children[i].id);
			    };
			
			    
			    for (var i = 0; i < self.mainData.sku.length; i++) {
			      if(self.mainData.sku[i].sku_item.indexOf(parseInt(id))!=-1){
			        self.choose_sku_item.push.apply(self.choose_sku_item,self.mainData.sku[i].sku_item);  
			      };
			    };
			
			    for(var i=0;i<self.sku_item.length;i++){
			      if(self.choose_sku_item.indexOf(parseInt(self.sku_item[i]))==-1){
			        self.sku_item.splice(i, 1); 
			      };
			    };
			    self.sku_item.push(parseInt(id));
				console.log('self.sku_item',self.sku_item)
			    for(var i=0;i<self.mainData.sku.length;i++){ 
			      if(JSON.stringify(self.mainData.sku[i].sku_item.sort())==JSON.stringify(self.sku_item.sort())){
			        //self.id = self.mainData.sku[i].id;
			        self.skuData = self.$Utils.cloneForm(self.mainData.sku[i]);
					self.specsCurr = i
					console.log('self.specsCurr',self.specsCurr)
			      };   
			    }; 
			},
			
			wxJsSdk() {
				const self = this;
				const postData = {
					thirdapp_id: 2,
					url: location.href.split('#')[0]
				};
				const callback = (res) => {
					self.$jweixin.config({
						debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
						appId: res.appId, // 必填，公众号的唯一标识
						timestamp: res.timestamp, // 必填，生成签名的时间戳
						nonceStr: res.nonceStr, // 必填，生成签名的随机串
						signature: res.signature, // 必填，签名
						jsApiList: ['scanQRCode','previewImage'] // 必填，需要使用的JS接口列表
					});
					self.$jweixin.ready(function() { //需在用户可能点击分享按钮前就先调用
					});
					self.$jweixin.error(function(res) {
						console.log('error', res)
					});
					self.$Utils.finishFunc('wxJsSdk');
					console.log(self.$jweixin)
				};
				self.$apis.WxJssdk(postData, callback);
			},
			
			getImg($event){
				
			},
			
			getUrl(url){
				const self = this;
				self.param = [url];
				var param = url;
				console.log(param);
				self.$refs.previewImage.open(param); // 传入当前选中的图片地址或序号
			},
			
			toCar(){
				const self = this;
				if(!self.isVip){
					self.is_show1 = true;
					return
				};
				self.Router.redirectTo({route:{path:'/pages/shop-car/shop-car'}})
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

			goBuy() {
				const self = this;
				if (!self.mainData.sku[self.specsCurr]) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('商品暂无可选规格！', 'none');
					return
				};
				uni.setStorageSync('canClick', false);
				self.orderList.push({
					sku_id: self.mainData.sku[self.specsCurr].id,
					count: self.num,
					type: self.mainData.type,
					product: self.mainData,
					skuIndex: self.specsCurr
				}, );
				
				uni.setStorageSync('payPro', self.orderList);
				self.gg_show = !self.gg_show
				self.Router.navigateTo({
					route: {
						path: '/pages/shop-order/shop-order'
					}
				})
				uni.setStorageSync('canClick', true);
			},

			addCar() {
				const self = this;
				if(!self.isVip){
					self.is_show1 = true;
					return
				};
				var obj = self.mainData;
				self.mainData.skuIndex = self.specsCurr;
				self.mainData.skuId = self.mainData.sku[self.specsCurr].id;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if (array[i].skuId == self.mainData.sku[self.specsCurr].id) {
						var target = array[i]
					}
				}

				console.log(target)
				if (target) {
					target.count = target.count + 1
				} else {
					console.log(111)
					var target = self.mainData;
					target.count = self.num;
					target.isSelect = true;
				}
				self.$Utils.showToast('加入成功', 'none');
				self.$Utils.setStorageArray('cartData', target, 'skuId', 999);
			},

			ggShow() {
				const self = this;
				if(!self.isVip){
					self.is_show1 = true;
					return
				};
				self.gg_show = !self.gg_show
			},

			changeSku(index) {
				const self = this;
				self.specsCurr = index;
			},

			count(i) {
				const self = this;
				if (self.num + i > 0) {
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
	page {
		background-color: #f5f5f5;
	}
</style>
<style scoped>
	.banner .swiper-box {
		width: 750rpx;
		height: 750rpx;
	}

	.banner image {
		width: 750rpx;
		height: 750rpx;
	}

	.gg-icon {
		width: 38rpx;
		height: 10rpx;
	}

	.detailImg {
		width: 750rpx;
		height: 3950rpx;
	}

	.fh-icon {
		width: 41rpx;
		height: 33rpx;
		margin: auto;
	}

	.car-icon {
		width: 38rpx;
		height: 38rpx;
		margin: auto;
	}

	.btnBox {
		width: 460rpx;
	}

	.btn {
		width: 220rpx;
		line-height: 80rpx;
		background-color: #6F7178;
		margin: 20rpx 0;
		border-radius: 40rpx;
	}

	.btn:nth-child(2) {
		background-color: #F59824;
	}

	.ggBox {
		min-height: 900rpx;
	}

	.gg {
		width: 182rpx;
	}

	.gg image {
		width: 182rpx;
		height: 182rpx;
		border-radius: 10rpx;
	}

	.span {
		background-color: #f5f5f5;
		padding: 0 20rpx;
		margin-right: 30rpx;
		margin-bottom: 20rpx;
		display: inline-block;
		line-height: 56rpx;
		border-radius: 28rpx;
	}

	.ggPart .on {
		background-color: #FFF7ED;
		color: #E39423;
		border: 1px solid #E39423;
	}

	.ggBtn {
		line-height: 80rpx;
		background-color: #6F7178;
		width: 330rpx;
		border-radius: 40rpx;
		margin: 20rpx 0;
	}

	.ggBtn:nth-child(2) {
		background-color: #F59824;
	}

	.ggPartBox {
		max-height: 600rpx;
	}

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
