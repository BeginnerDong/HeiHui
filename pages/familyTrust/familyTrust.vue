<template>
	<view>
		
		<view class="p-r"  @click="Router.navigateTo({route:{path:'/pages/serach/serach?type=3'}})">
			<input type="text" 
			 placeholder="搜索你想找的" class="ss"/>
			<image src="../../static/images/search-icon.png" class="ss-icon p-a"></image>
		</view>
		
		<view class="list flexX">
			<view class="li" :class="liCurr==index?'on':''" @click="changeLi(index,item.id)"
			v-for="(item,index) in labelData" :key="index"
			:style="{width: 100/labelData.length+'%'}">{{item.title}}</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<block v-for="(item,index) in topData" :key='index'>
			<view class="mx-3 mt-3 p-r bB-f5" v-show="index == 0"
			@click="isShow(item.id)">
				<image :src="item&&item.mainImg&&item.mainImg[0]&&item.mainImg[0].url" class="trustImg"></image>
				<view class="font-32 colorf p-1 p-a bottom-0 jbTit">{{item.title}}</view>
			</view>
		</block>
		
		<view class="article mx-3 radius10 py-3 flex1 bB-f5" v-for="(item,index) in mainData" :key="index"
		@click="isShow(item.id)">
			<view class="flex5 flex-1 conTxt">
				<view class="tit font-32 avoidOverflow2 flex-1">{{item.title}}</view>
				<view class="font-24 color9 pt-2">
					<text class="artSgin">#{{item.label[item.menu_id].title}}</text> {{item.create_time}}
				</view>
			</view>
			<image :src="item&&item.mainImg&&item.mainImg[0]&&item.mainImg[0].url" class="artImg"></image>
		</view>
		
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
				<view class="txt">很抱歉，您还不是恒辉财富会员</view>
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
				topData:[],
				mainData:[],
				searchItem:{
					type: 3,
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
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/familyTrust/familyTrust')
			self.$Utils.loadAll(['getLabelData','getKefuData','getNoteData'], self);
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
							self.$Router.navigateTo({
								route: {
									path: '/pages/detail/detail?type=3&id=' + self.willId
								}
							})
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
			
			isShow(id) {
				const self = this;
				console.log(id)
				self.willId = id;
				if(self.isVip){
					self.$Router.navigateTo({
						route: {
							path: '/pages/detail/detail?type=3&id=' + id
						}
					})
				}else{
					if(self.userInfoData.deadline>0&&self.userInfoData.deadline>Date.parse(new Date())/1000){
						self.$Router.navigateTo({
							route: {
								path: '/pages/detail/detail?type=3&id=' + id
							}
						})
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
			
			changeLi(index,id){
				const self = this;
				self.liCurr = index;
				self.searchItem.menu_id = id;
				self.getMainData(true,id);
			},
			
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 1,
					parentid : 5
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData =  res.info.data;
					}
					console.log('label',self.labelData);
					self.searchItem.menu_id = self.labelData[0].id;
					self.getMainData(true,self.labelData[0].id);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew,id) {
				const self = this;
				if (isNew) {
					self.topData = [];
					self.mainData = [];
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						var data = res.info.data;
						for(var i=0;i<data.length;i++){
							data[i].create_time = data[i].create_time.substring(0,10);
							if(data[i].top === 1 && data[i].menu_id == id){
								self.topData.push(data[i])
							}else if(data[i].top === 0){
								self.mainData.push(data[i])
							}
						}
						// console.log('top',self.topData);
						// console.log('main',self.mainData);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>
<style>page{background-color: #fff!important;}</style>
<style scoped>
.li{padding: 0 30rpx;flex-shrink: 0;}
.trustImg{width: 690rpx;height: 340rpx;}
.artImg{width: 175rpx;height: 140rpx;}
.conTxt{height: 140rpx;}
.article .tit{width: 455rpx;}
.tc{width: 620rpx;margin: auto;left: 0;right: 0;top: 30%;}
.tc .txt{padding: 60rpx;}
.tc .btn{line-height: 100rpx;}
</style>
