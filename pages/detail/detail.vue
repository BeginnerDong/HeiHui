<template>
	<view class="px-3">
		
		<view class="font-36 pt-3">{{mainData.title}}</view>
		<view class="font-24 color6 flex1 mt-1 pb-4 bB-e1">
			<view class="flex">
				<image src="../../static/images/details-icon.png" class="time-icon"></image>
				<view>{{mainData.publish_time}}</view>
			</view>
			<view class="flex" @click="Utils.stopMultiClick(collect)">
				<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/details-icon2.png':'../../static/images/details-icon1.png'" class="sc-icon"></image>
				<view>{{mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'已收藏':'收藏'}}</view>
			</view>
		</view>
		
		<view class="pt-3">
			<video  :src="mainData&&mainData.bannerImg&&mainData.bannerImg[0]&&mainData.bannerImg[0].url"
			v-show="mainData&&mainData.bannerImg&&mainData.bannerImg[0]&&mainData.bannerImg[0].url"
			controls></video>
			<view class="pt-3 font-26">
				
				<div class="content ql-editor"   @click="getImg($event)" style="padding:0" v-html="mainData.content" >
					
					
				</div>
			</view>
		</view>
		
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">
					<view class="content ql-editor text-center" style="padding:0;" v-html="tipsData.content">
						
					</view>
				</view>
				<view class="" style="margin-bottom: 30rpx;color: #888;"  @click="phoneCall()">联系客服：{{kefuPhone}}</view>
				<view class="flex1 bT-f5 btn">
					<view class="w-50 bR-f5" @click="showToast">我知道了</view>
					<view class="colorM w-50" @click="phoneCall()">联系客服</view>
				</view>
			</view>
		</view>
		
		<template>
		    <view>
		        <previewImage ref="previewImage" :opacity="1" :imgs="param" :height="height"></previewImage>
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
				mainData:{},
				searchItem:{
					thirdapp_id: 2
				},
				Utils:this.$Utils,
				self:this,
				is_show:false,
				kefuData:{},
				param:[],
				height:0,
				tipsData:{},
				kefuPhone:''
			}
		},
		onLoad(option){
			const self = this;
			window.getImg = function($event){
				console.log($event);
				/* self.height = $event.target.height;
				self.param = [$event.target.currentSrc];
				var param = $event.target.currentSrc;
				console.log(param);
				self.$refs.previewImage.open(param); // 传入当前选中的图片地址或序号 */
				self.$jweixin.previewImage({
					current:0,
					urls:[$event.target.currentSrc]
				})
			};
			var options = self.$Utils.getHashParameters();
			console.log(options);
			self.searchItem.type = options[0].type;
			self.searchItem.id = options[0].id;
			//self.searchItem.menu_id = options[0].menu_id;
			console.log('option',option)
			self.$Utils.loadAll(['wxJsSdk','getMainData','getUserInfoData','getKefuData','getTipsData'], self);
		},
		
		methods: {
			
			getTipsData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'普通会员收藏提示',
					menu_id: 10
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.tipsData = res.info.data[0]
					}
					console.log('menu',self.tipsData)
					self.$Utils.finishFunc('getTipsData');
				}
				self.$apis.articleGet(postData, callback);
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
			
			showToast(){
				const self = this;
				self.is_show = false
			},
			
			getKefuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id: 3,
					thirdapp_id: 2
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.kefuData = res.info.data[0];
					}
					self.$Utils.finishFunc('getKefuData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getImg($event){
				
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
						if(self.userInfoData.behavior==1||self.userInfoData.user_type==1){
							self.isVip = true
						};
						if(res.info.data[0].staff&&res.info.data[0].staff[0]&&res.info.data[0].staff[0].info&&res.info.data[0].staff[0].info.phone!=''){
							self.kefuPhone = res.info.data[0].staff[0].info.phone
						}else{
							self.kefuPhone = uni.getStorageSync('user_info').thirdApp.phone
						}
						self.$Utils.finishFunc('getUserInfoData');
					}
				}
				self.$apis.userGet(postData, callback);
			},
			
			
			
			collect() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(!self.isVip){
					self.is_show = true;
					uni.setStorageSync('canClick', true);
					return
				};
				if (self.mainData.log.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '收藏成功',
					relation_id: self.mainData.id,
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				if(self.searchItem.type==2){
					postData.data.behavior=2
				}else{
					postData.data.behavior=1
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.log.push({
							status: 1,
							id: res.info.id
						});
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData.log[0].id
					},
					data: {
						status: -self.mainData.log[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.log[0].status = -self.mainData.log[0].status;
			
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					log:{
						token:uni.getStorageSync('user_token'),
						
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				if(self.searchItem.type==2){
					postData.getAfter.log.searchItem.behavior=2
				}else{
					postData.getAfter.log.searchItem.behavior=1
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data[0];
						self.mainData.publish_time = self.$Utils.timeto(self.mainData.publish_time,'ymd-hms').substr(0,10)
						/* self.mainData.create_time = self.mainData.create_time.substr(0,10); */
						//self.mainData.content = self.mainData.content.replace(/src/g, 'data-src')
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img onclick='self.getImg(event)'"`);
						console.log('main',self.mainData);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			phoneCall(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber:self.kefuPhone
				})
			},
			
		}
	}
</script>

<style scoped>
video{width: 690rpx;height: 360rpx;}

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
	.tc{width: 620rpx;margin: auto;left: 0;right: 0;top: 30%;}
	.tc .txt{padding: 60rpx;}
	.tc .btn{line-height: 100rpx;}
	
	.mhBox{filter: blur(1px);}
	.mh{filter: blur(6px);}
</style>
