<template>
	<view>
		<view v-show="nowData&&nowData.mainImg&&nowData.mainImg[0]&&nowData.mainImg[0].url">
			<image :src="nowData&&nowData.mainImg&&nowData.mainImg[0]&&nowData.mainImg[0].url" class="appoinImg"></image>
		</view>
		
		<view class="flex py-2 px-3">
			<image src="../../static/images/about-icon.png" class="app-icon"></image>
			<view class="font-32 font-w">最新视频</view>
		</view>
		<view class="flex px-3">
			<view style="position: absolute;width: 260rpx;height: 180rpx;left: 30rpx;z-index: 999;" @click="isShow(nowData)"></view>
			<video :src="nowData&&nowData.bannerImg&&nowData.bannerImg[0]&&nowData.bannerImg[0].url" 
			x5-video-orientation="landscape" x5-video-player-fullscreen="true"
			class="spImg" controls=""
			v-if="nowData&&nowData.bannerImg&&nowData.bannerImg[0]&&nowData.bannerImg[0].url"></video>
			<image src="../../static/images/null1.png" v-else class="spImg"></image>
			<view class="pl-2 flex-1 flex5 spTxt">
				<view class="avoidOverflow2 flex-1">主题：{{nowData.title}}</view>
				<view class="pb-1">时间：{{nowData.create_time}}</view>
				<view>嘉宾：{{nowData.small_title}}</view>
			</view>
		</view>
		<view class="font-24 px-3 pt-3">
			<view class="content ql-editor" style="padding:0;" v-html="nowData.content">
				
			</view>
		</view>
		
		<view class="flex py-2 px-3">
			<image src="../../static/images/about-icon.png" class="app-icon"></image>
			<view class="font-32 font-w">往期视频回放</view>
		</view>
		<view class="font-24 line-h flex2 pb-4">
			<view class="flex4" @click="changeLi(0,labelData[0].id)">
				<image src="../../static/images/baike-icon3.png" class="app-icon1" v-show="liCurr!=0"></image>
				<image src="../../static/images/baike-icon4.png" class="app-icon1" v-show="liCurr==0"></image>
				<view>资产配置</view>
			</view>
			<view class="flex4" @click="changeLi(1,labelData[1].id)">
				<image src="../../static/images/baike-icon2.png" class="app-icon1" v-show="liCurr!=1"></image>
				<image src="../../static/images/baike-icon5.png" class="app-icon1" v-show="liCurr==1"></image>
				<view>家族财富管理</view>
			</view>
			<view class="flex4" @click="changeLi(2,labelData[2].id)">
				<image src="../../static/images/baike-icon1.png" class="app-icon1" v-show="liCurr!=2"></image>
				<image src="../../static/images/baike-icon6.png" class="app-icon1" v-show="liCurr==2"></image>
				<view>其他</view>
			</view>
		</view>
		
		<view class="flex1 mx-3 pb-3 p-r"
		v-for="(item,index) in mainData" :key="index"
		@click="isShow(item)">
			<image :src="item&&item.mainImg&&item.mainImg[0]&&item.mainImg[0].url?item.mainImg[0].url:'../../static/images/null.png'" class="spImg"></image>
			<view class="pl-2 py-1 flex5 wqTxt">
				<view class="font-30 font-w avoidOverflow2">{{item.title}}</view>
				<view class="font-26 pt-3 avoidOverflow2">简介：{{item.description}}</view>
			</view>
			<view class="font-22 p-1 Mgb colorf line-h p-a top-0 left-0">嘉宾 | {{item.small_title}}</view>
		</view>
		
		
		<!-- 弹框 -->
		<!-- 重要提示 -->
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-r overflow-h tips">
				<image src="../../static/images/product-icon8.png" class="x-icon" @click="close()"></image>
				<view class="flex0 font-30 font-w pb-5 pt-3 mx-3">
					<image src="../../static/images/product-icon9.png" class="tips-icon"></image>
					<view>重要提示</view>
				</view>
				<view class="font-26 pt-2 mx-3">
					<view class="content ql-editor" style="padding:0;" v-html="noteData.content">
						
					</view>
				</view>
				<view class="p-a bottom-0 left-0 right-0">
					<view class="colorf font-24 pb-1">
						<image src="../../static/images/product-icon10.png" class="tips-icon1"></image>
						<view class="time">{{text}}</view>
					</view>
					<view class="tipBtn" @click="toDetail()" :style="hasView?'background:#E39423':'background:#999'">确定</view>
				</view>
			</view>
		</view>
		
		<view class="bg-mask" v-show="is_show1">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">
					<view class="content ql-editor text-center" style="padding:0;" v-html="tipsData.content">
						
					</view>
				</view>
				<view class="" style="margin-bottom: 30rpx;color: #888;">联系客服：{{kefuData.phone}}</view>
				<view class="flex1 bT-f5 btn">
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
				liCurr:0,
				
				nowData:{},
				mainData:[],
				searchItem:{
					type: 6,
					thirdapp_id: 2
				},
				labelData:[],
				is_show: false,
				is_show1:false,
				kefuData:{},
				isVip:false,
				noteData:{},
				text:'倒计时10秒',
				currentTime:10,
				hasView:false,
				tipsData:{}
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/appointment/appointment')
			self.$Utils.loadAll(['getLabelData','getTipsData'], self);
		},
		
		onShow() {
			const self = this;
			self.getUserInfoData()
		},
		
		methods: {
			
			toDetail(){
				const self = this;
				if(self.hasView){
					self.is_show = false;
					self.text = '倒计时10秒';
					const postData = {};
					postData.tokenFuncName = 'getUserToken';
					postData.data = {
						deadline:Date.parse(new Date())/1000 + uni.getStorageSync('user_info').thirdApp.temporary*60 
					};
					var callback = function(res) {
						if (res.solely_code == 100000) {
							/* self.$Router.navigateTo({
								route: {
									path: '/pages/detail/detail?type=3&id=' + self.willId
								}
							}) */
							uni.setStorageSync('appointmentData',self.willItem)
							self.Router.navigateTo({route:{path:'/pages/appointmentDetail/appointmentDetail'}})
						}
					};
					self.$apis.userInfoUpdate(postData, callback);
				}
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if(self.userInfoData.behavior==1){
							self.isVip = true
						};
						
					}
				}
				self.$apis.userInfoGet(postData, callback);
			},
			
			isShow(item) {
				const self = this;
				console.log(item)
				self.willItem = item;
				if(self.isVip){
					/* self.$Router.navigateTo({
						route: {
							path: '/pages/detail/detail?type=3&id=' + id
						}
					}) */
					uni.setStorageSync('appointmentData',self.willItem)
					self.Router.navigateTo({route:{path:'/pages/appointmentDetail/appointmentDetail'}})
				}else{
					if(self.userInfoData.deadline>0&&self.userInfoData.deadline>Date.parse(new Date())/1000){
						/* self.$Router.navigateTo({
							route: {
								path: '/pages/detail/detail?type=3&id=' + id
							}
						}) */
						uni.setStorageSync('appointmentData',self.willItem)
						self.Router.navigateTo({route:{path:'/pages/appointmentDetail/appointmentDetail'}})
					}else if(self.userInfoData.deadline>0&&self.userInfoData.deadline<Date.parse(new Date())/1000){
						self.is_show1 = true;
						return
					}else if(self.userInfoData.deadline==0){
						self.hasView = false;
						self.interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
							self.text='倒计时'+self.currentTime + 's';//按钮文字变成倒计时对应秒数
							self.hasView = false;
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(self.interval)
								
								self.hasView = true;
								self.text='可以查看啦';
								self.currentTime= 10;
							}
						}, 1000);
						self.is_show = !self.is_show
					}
					
				}	
			},
			
			getTipsData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'会员提示',
					menu_id: 10
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.tipsData = res.info.data[0]
					}
					self.$Utils.finishFunc('getTipsData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getNoteData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'重要提示'
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
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
				var callback = function(res){
					if(res.info.data.length > 0){
						self.kefuData = res.info.data[0];
					}
					self.$Utils.finishFunc('getKefuData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			phoneCall(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber:self.kefuData.phone
				})
			},
			
			showToast1(){
				const self = this;
				self.is_show1 = false
			},
			
			close(){
				const self = this;
				self.is_show = false
				clearInterval(self.interval)
			},
			
			goDetail(item){
				const self = this;
				uni.setStorageSync('appointmentData',item)
				self.Router.navigateTo({route:{path:'/pages/appointmentDetail/appointmentDetail'}})
			},
			
			changeLi(i,id){
				const self = this;
				self.liCurr = i;
				self.getMainData(true,id);
			},
			
			
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parentid : 9
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData =  res.info.data;
					}
					console.log('label',self.labelData);
					// self.searchItem.menu_id = self.labelData[0].id;
					self.getMainData(true,self.labelData[0].id);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew,id) {
				const self = this;
				if (isNew) {
					self.nowData = [];
					self.mainData = [];
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						var data = res.info.data;
						console.log('data',data)
						for(var i=0;i<data.length;i++){
							data[i].create_time = data[i].create_time.substring(0,16);
							if(data[i].top == 1){
								self.nowData = data[i]
							}else if(data[i].top == 0 &&  data[i].menu_id == id){
								self.mainData.push(data[i])
							}
						}
						console.log('top',self.nowData);
						console.log('main',self.mainData);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
.appoinImg{width: 100%;height: 400rpx;}
.app-icon{width: 30rpx;height: 31rpx;margin-right: 10rpx;}
.app-icon1{width: 110rpx;height: 110rpx;margin-bottom: 30rpx;}
.spImg{width: 260rpx;height: 180rpx;}
.spTxt{height: 180rpx;}
.spTxt view{width: 410rpx;}
.wqTxt view{width: 408rpx;}
.wqTxt{height: 180rpx;}

.tcBox{margin-top: 40%;overflow: hidden;}
.tcBox .tit{line-height: 80rpx;}
.tcBox input{width: 600rpx;height: 80rpx;font-size: 26rpx;margin: 0 auto 30rpx;border: 1px solid #e1e1e1;border-radius: 10rpx;}
.btn600{line-height: 70rpx;}
.tc{width: 620rpx;margin: auto;left: 0;right: 0;top: 30%;}
.tc .txt{padding: 60rpx;}
.tc .btn{line-height: 100rpx;}
</style>
