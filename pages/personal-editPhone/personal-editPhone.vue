<template>
	<view>
		
		<view v-show="is_show==0">
			<image src="../../static/images/replace the phone-icon1.png" class="phone-icon1"></image>
			<view class="text-center">绑定手机号：{{userData.login_name?userData.login_name:''}}</view>
			<view class="btn600" @click="isShow(1)">更换手机号</view>
		</view>
		
		<view v-show="is_show==1">
			<image src="../../static/images/replace the phone-icon.png" class="phone-icon"></image>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2">
				<input type="number" v-model="phone" placeholder="请输入新手机号" />
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" >
				<input type="number" v-model="code" placeholder="请输入验证码" />
				<view class="colorM font-w" @click="sendCode()" v-if="!hasSend">{{text}}</view>
				<view class="colorM font-w" v-if="hasSend">{{text}}</view>
			</view>
			<view class="btn600" @click="changePhone()">完成</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				is_show:0,
				userData:{},
				phone:'',
				code:'',
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
		},
		
		methods: {
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.phone;
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					return;
				}
				var postData = {
					data:{
						phone:self.phone,
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
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				var callback = function(res){
					if(res.info.data.length > 0){
						self.userData = res.info.data[0];
						self.is_show = 0
					}
					self.$Utils.finishFunc('getUserData');
				}
				self.$apis.userGet(postData, callback);
			},
			
			changePhone() {
				const self = this;
				if(self.phone==''){
					self.$Utils.showToast('请输入新手机号', 'none')
					return
				};
				if(self.code==''){
					self.$Utils.showToast('请输入验证码', 'none')
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					login_name:self.phone
				};
				postData.smsAuth = {
					phone:self.phone,						
					code:self.code,
				};
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						data: {
							phone:self.phone
						},
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						}
					}
				];	
				var callback = function(res){
					if(res.solely_code == 100000){
						self.getUserData()
					}else{
						self.$Utils.showToast(res.msg, 'none', 1000)
					}
				}
				self.$apis.userUpdate(postData, callback);
			},
			
			isShow(i){
				const self = this;
				self.is_show = i
			}
		}
	}
</script>

<style scoped>
.phone-icon{height: 222rpx;width: 138rpx;margin: 100rpx auto 70rpx;}
.phone-icon1{height: 222rpx;width: 174rpx;margin: 100rpx auto 70rpx;}
.btn600{margin-top: 100rpx;}

input{font-size: 26rpx;flex: 1;}
.xx{width: 600rpx;margin: 0 auto;}
</style>
