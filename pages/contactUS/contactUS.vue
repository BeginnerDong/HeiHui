<template>
	<view class="px-3 pt-2">
		
		<view class="map mb-2 shadowM">
			<!-- <image src="../../static/images/contactus-img.png" mode=""></image> -->
			<map style="width:690rpx;height: 500rpx;" v-if="markers[0].latitude" :latitude="mainData.latitude" :longitude="mainData.longitude" :markers="markers"></map>
		</view>
		
		<view class="flex1 py-3 pl-3 shadowM line-h mb-3">
			<image src="../../static/images/contactus-icon.png" class="us-icon"></image>
			<view class="flex-1 pl-3">
				<view class="font-30 color6 pb-2" style="font-weight: 700;">联系电话</view>
				<view>{{mainData.phone}}</view>
			</view>
		</view>
		<view class="flex1 py-3 pl-3 shadowM line-h mb-3">
			<image src="../../static/images/contactus-icon1.png" class="us-icon"></image>
			<view class="flex-1 pl-3">
				<view class="font-30 color6 pb-2" style="font-weight: 700;">联系地址</view>
				<view>{{mainData.description}}</view>
			</view>
		</view>
		<!-- <view class="flex3 py-3 pl-4 shadowM line-h mb-3">
			<image src="../../static/images/contactus-icon2.png" class="us-icon1"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">意见反馈</view>
				<textarea v-model="data.content" placeholder="请输入" />
				<view class="btn540 mt-2" @click="submit">提交</view>
			</view>
		</view> -->
		<view class="p-r item">
			<!-- <view class="flex0 pt-3 z-index10 p-r pb-1">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">意见反馈</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view> -->
			
			<view class=" p-3 p-r shadowM radius10 contact">
				<view class="d-flex">
					<image src="../../static/images/contactus-icon2.png" style="height: 42rpx;" class="us-icon"></image>
					<view class="font-30 color6 pb-2 pl-3" style="font-weight: 700;">意见反馈</view>
				</view>
				
				<view class="flex bB-f5 py-2">
					<image src="../../static/images/jianjie-icon4.png" class="con-icon"></image>
					<input type="text" v-model="submitData.title" placeholder="姓名" />
				</view>
				<view class="flex bB-f5 py-2 mt-3">
					<image src="../../static/images/jianjie-icon3.png" class="con-icon1"></image>
					<input type="text" v-model="submitData.phone" placeholder="手机号" />
				</view>
				<view class="flex bB-f5 py-2 mt-3">
					<image src="../../static/images/jianjie-icon2.png" class="con-icon2"></image>
					<input type="text" v-model="submitData.content" placeholder="留言框" />
				</view>
				<view class="btn600" @click="successSubmit">确定</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				submitData:{
					title:'',
					phone:'',
					content:''
				},
				userData:{},
				markers:[
					{
						iconPath:'../../static/images/dingwei.png'
					},
				]
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/contactUS/contactUS')
			self.$Utils.loadAll(['getMainData','getUserData'], self);
		},
		methods: {
			
			successSubmit(){
				const self = this;
				if(self.submitData.title == ''){
					self.$Utils.showToast('请输入姓名','none')
				}else if(self.submitData.phone == ''){
					self.$Utils.showToast('请输入电话号码','none')
				}else if(self.submitData.content == ''){
					self.$Utils.showToast('请输入内容','none')
				}else{
					var reg = /^1[3456789]\d{9}$/
					if(reg.test(self.submitData.phone)){
						self.submit()
					}else{
						self.$Utils.showToast('电话号码格式错误','none')
					}
				}
			},
			
			submit(){
				const self = this;
				const postData = {
					data:self.submitData
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '谢谢您的反馈',
						    duration: 2000,
						});
						setTimeout(function(){
							self.submitData.title = '';
							self.submitData.phone = '';
							self.submitData.content = '';
						},2000)
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				var callback = function(res){
					if(res.info.data.length > 0){
						self.userData = res.info.data[0];
						self.submitData.title = self.userData.info.name;
						self.submitData.phone = self.userData.info.phone
					}
					self.$Utils.finishFunc('getUserData');
				}
				self.$apis.userGet(postData, callback);
			},
			
			/* submit(){
				const self = this;
				if(self.data.content==''){
					self.$Utils.showToast('请输入内容','none');
					return
				};
				const postData = {
					data:self.data
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '已提交申请',
						    duration: 2000,
						});
						setTimeout(function(){
							uni.navigateBack({
							    delta: 1
							});
						},2000)
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.messageAdd(postData, callback);
			}, */
			
			
			
			getMainData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					menu_id: 3,
					thirdapp_id: 2
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data[0];
						self.mainData.latitude = parseFloat(self.mainData.latitude);
						self.mainData.longitude = parseFloat(self.mainData.longitude);
						self.markers[0].latitude = self.mainData.latitude;
						self.markers[0].longitude = self.mainData.longitude;
					}
					self.$Utils.finishFunc('getMainData');
					console.log(self.markers)
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
.map{height: 500rpx;width: 690rpx;}
.map image{width: 100%;height: 100%;}
textarea{width: 540rpx;height: 340rpx;background-color: #f5f5f5;font-size: 22rpx;padding: 20rpx;box-sizing: border-box;margin: 0;}

.img{height: 428rpx;width: 100%;}
.bg-txt{width: 690rpx;height: 468rpx;position: absolute;top: 70rpx;;left: 0;right: 0;margin: 0 auto;}
.item{margin-bottom: 30rpx;}
.img1{width: 600rpx;height: 340rpx;margin: 30rpx auto 0;}
.img2{width: 200rpx;height: 130rpx;margin-bottom: 20rpx;margin-right: 19rpx;}
.img2:nth-child(3n){margin-right: 0;}
.img3{width: 100%;height: 200rpx;margin-top: 100rpx;}
.con-icon{width: 26rpx;height: 30rpx;}
.con-icon1{width: 23rpx;height: 30rpx;}
.con-icon2{width: 28rpx;height: 28rpx;}
.contact input{font-size: 26rpx;color: #666;padding-left: 30rpx;flex: 1;}
.btn600{width: 630rpx;margin-top: 50rpx;}
</style>
