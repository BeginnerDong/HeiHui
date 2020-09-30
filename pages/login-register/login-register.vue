<template>
	<view class="login font-26 color9">
		
		<image src="../../static/images/the login-img1.png" class="loginBg"></image>
		
		<image src="../../static/images/the login-img.png" class="logo"></image>
		
		 <view class="line-h">
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" v-show="is_show==1">
				<image src="../../static/images/the login-icon.png" class="xm-icon"></image>
				<input type="text" value="" placeholder="姓名" v-model="submitData.name"/>
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2">
				<image src="../../static/images/the login-icon1.png" class="sj-icon"></image>
				<input type="text" value="" placeholder="手机号或员工登录名" v-model="submitData.phone"/>
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" v-show="is_show==0 || is_show==1">
				<image src="../../static/images/the login-icon2.png" class="mm-icon"></image>
				<input type="password" value="" placeholder="密码" v-model="submitData.password"/>
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2"  v-show="is_show==2">
				<image src="../../static/images/the login-icon3.png" class="aq-icon"></image>
				<input type="number"  placeholder="验证码" v-model="code"/>
				<view class="color2 font-w" @click="sendCode()" v-if="!hasSend">{{text}}</view>
				<view class="color2 font-w" v-if="hasSend">{{text}}</view>
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" v-show="is_show==2">
				<image src="../../static/images/the login-icon2.png" class="mm-icon"></image>
				<input type="password" value="" placeholder="新密码" v-model="submitData.newPassword"/>
			</view>
		</view>
		
		<view class="btn600" @click="submit">确定</view>
		
		<!-- 登录显示 -->
		<view class="flex0 pt-5" v-show="is_show==0">
			<view class="d-flex a-center pr-5 bR-f5" @click="isShow(1)">
				注册账号
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
			<view class="d-flex a-center pl-5" @click="isShow(2)">
			  重置密码
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
		</view>
		<view class="flex0 pt-5" v-show="is_show==0">
			<view class="d-flex a-center">
			
				登录即代表同意<span style="color: #E39423;" @click="showModel">《{{artData.title}}》</span>
			</view>
		</view>
		<!-- 注册显示 -->
		<view class="flex0 pt-5" v-show="is_show==1">
			<view class="d-flex a-center" @click="isShow(0)">
				已有账号
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
		</view>
		
		<view class="flex0 pt-5" v-show="is_show==1">
			<view class="d-flex a-center">
				<image @click="change" style="margin-right: 20rpx;height: 30rpx;width: 30rpx;" :src="isAgree?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'"></image>
				注册须阅读并同意<span style="color: #E39423;" @click="showModel">《{{artData.title}}》</span>
			</view>
		</view>
		<view class="bg-mask" v-show="showToast">
			<view class="bg-white p-r overflow-h tips">
				<image src="../../static/images/product-icon8.png" class="x-icon" style="margin: 20px;" @click="showModel()"></image>
				<view class="flex0 font-30 font-w pb-3 pt-3 a-center" style="border-bottom: 1px solid #E5E5E5;">
					<image src="../../static/images/product-icon9.png" class="tips-icon"></image>
					<view>{{artData.title}}</view>
				</view>
				<view class="font-26 pt-2 mx-3" style="overflow-y: auto;height: 500px;padding-bottom: 150px;">
					<view class="content ql-editor" style="padding:0;" v-html="artData.content">
						
					</view>
				</view>
				<view class="p-a bottom-0 left-0 right-0">
					<!-- <view class="colorf font-24 pb-1">
						<image src="../../static/images/product-icon10.png" class="tips-icon1"></image>
						<view class="time">{{text}}</view>
					</view> -->
					<view class="tipBtn" @click="showModel()" style="background:#E39423">确定</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import token from '@/common/token.js';
	export default {
		data() {
			return {
				is_show:0,
				submitData:{
					name:'',
					phone:'',
					password:'',
					thirdapp_id:2,
					newPassword:''
				},
				code:'',
				Router:this.$Router,
				Utils:this.$$Utils,
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
				isAgree:false,
				showToast:false,
				artData:{}
			}
		},
		
		
		onShow() {
			const self = this;
			var param = self.$Utils.getHashParameters()[0];
			self.$Utils.loadAll(['getArtData'], self);
			if(param.code){
				return
			}else{
				var href = 'http://szegam.com/h5'
				window.location.href = 'https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx700af4c0583be8ab&redirect_uri='+
				encodeURIComponent(href)+'&response_type=code&scope=snsapi_userinfo';
			}
			
			
		},
		
		methods: {
			
			change(){
				const self = this;
				self.isAgree = !self.isAgree
			},
			
			showModel(){
				const self = this;
				self.showToast = !self.showToast
			},
			
			getArtData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: 49,
					thirdapp_id: 2
				};
				postData.noLoading = true
				var callback = function(res){
					if(res.info.data.length > 0){
						self.artData = res.info.data[0];
					}
					self.$Utils.finishFunc('getArtData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.phone,
					}
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.$Utils.showToast('发送成功', 'none', 1000)
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
							}
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			reset() {
				const self = this;
				if(self.submitData.phone==''){
					self.$Utils.showToast('请输入手机号', 'none')
					return
				};
				if(self.code==''){
					self.$Utils.showToast('请输入验证码', 'none')
					return
				};
				if(self.submitData.newPassword==''){
					self.$Utils.showToast('新密码', 'none')
					return
				};
				const postData = {
					phone: self.submitData.phone,
					password:self.submitData.newPassword
				};
				postData.smsAuth = {
					phone:self.submitData.phone,						
					code:self.code,
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						console.log(res);
						self.$Utils.showToast('重置成功', 'none')
						setTimeout(function() {
							self.is_show = 0
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				}
				self.$apis.resetPassword(postData, callback);
			},
			
			submit(){
				const self = this;
				if(self.is_show==0){
					self.login()
				}else if(self.is_show==1){
					self.register()
				}else if(self.is_show==2){
					self.reset()
				}
			},
			
			isShow(i){
				const self = this;
				self.is_show = i;
			},
			
			login() {
				const self = this;
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.thirdapp_id;
				delete newObject.name;
				delete newObject.newPassword;
				var pass = self.$Utils.checkComplete(newObject);
				if(!pass){
					self.$Utils.showToast('请补全登录信息', 'none')
					return
				};
				var reg = /^1[3456789]\d{9}$/
				const postData = self.$Utils.cloneForm(newObject);
				self.$Utils.showToast('登陆中...', 'none')
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_info', res.info);
						self.$Utils.showToast('绑定微信中...', 'none')
						const c_callback = (c_res) => {
							console.log(c_res)
							if(c_res.solely_code==100000){
								setTimeout(function() {
									if(uni.getStorageSync('path')){
										self.Router.redirectTo({route:{path:uni.getStorageSync('path')}})
									}else{
										self.Router.redirectTo({route:{path:'/pages/introduction/introduction'}})
									}
								}, 1000);
							}
						};
						token.getWeixinToken({},c_callback)
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
				};
				self.$apis.loginByUser(postData, callback);
			},
			
			tokenGet() {
				const self = this;
				const postData = {
					searchItem: {
						user_no: 'U917130956611055'
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userData = res.info;
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_no', res.info.user_no);
						uni.setStorageSync('user_info', res.info);
						var time = parseInt(new Date().getTime()) + 3500000;
						uni.setStorageSync('token_expire_time',time);
					}
					console.log('res', res)
					self.$Utils.finishFunc('tokenGet');
				};
				self.$apis.tokenGet(postData, callback);
			},
			
			register() {
				const self = this;
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.newPassword;
				
				var pass = self.$Utils.checkComplete(newObject);
				if(!pass){
					self.$Utils.showToast('请补全注册信息', 'none')
					return
				};
				var reg = /^1[3456789]\d{9}$/
				if(!reg.test(self.submitData.phone)){
					self.$Utils.showToast('电话号码格式错误','none')
					return
				};
				if(!self.isAgree){
					self.$Utils.showToast('请勾选并同意"'+self.artData.title+'"','none')
					return
				};
				const postData = self.$Utils.cloneForm(newObject);
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.$Utils.showToast('注册成功，自动登陆中...', 'none');
						uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_info', res.info);
						self.$Utils.showToast('绑定微信中...', 'none')
						const c_callback = (c_res) => {
							console.log('c_res',c_res)
							if(c_res.solely_code==100000){
								setTimeout(function() {
									if(uni.getStorageSync('path')){
										self.Router.redirectTo({route:{path:uni.getStorageSync('path')}})
									}else{
										self.Router.redirectTo({route:{path:'/pages/introduction/introduction'}})
									}
								}, 1000);
							}
						};
						token.getWeixinToken({},c_callback)
						/* uni.setStorageSync('user_token', res.token);
						uni.setStorageSync('user_info', res.info);
						setTimeout(function() {
							if(uni.getStorageSync('path')){
								self.Router.redirectTo({route:{path:uni.getStorageSync('path')}})
							}else{
								self.Router.redirectTo({route:{path:'/pages/introduction/introduction'}})
							}
						}, 1000); */
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
				};
				self.$apis.register(postData, callback);
			},
		}
	}
</script>

<style>
.logo{width: 237rpx;height: 210rpx;margin: 0 auto;margin: 60rpx auto 80rpx;}
.login input{font-size: 26rpx;flex: 1;}
.xx{width: 600rpx;margin: 0 auto;}
.btn600{margin-top: 60rpx;}
.loginBg{width: 100%;height: 242rpx;position: fixed;bottom: 0;}
</style>
