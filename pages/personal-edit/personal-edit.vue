
<template>
	<view class="px-3 bg-white">
		
		<view class="flex1 py-3 bB-e1">
			<view class="color6 flex-1">头像</view>
			<image style="border-radius: 50%;overflow: hidden;" :src="userData.headImgUrl?userData.headImgUrl:'../../static/images/head.png'" class="userImg"></image>
			<!-- <image src="../../static/images/mall-icon3.png" class="R-icon"></image> -->
		</view>
		<view class="flex1 py-4 bB-e1">
			<view class="color6 flex-1">昵称</view>
			<view class="font-26">{{userData?userData.nickname:'暂未获取到昵称'}}</view>
			<!-- <image src="../../static/images/mall-icon3.png" class="R-icon"></image> -->
		</view>
		<view class="flex1 py-4 bB-e1" @click="Router.navigateTo({route:{path:'/pages/personal-editPhone/personal-editPhone'}})">
			<view class="color6 flex-1">电话</view>
			<view class="font-26">{{userData.login_name}}</view>
			<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				userData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
		},
		
		methods: {
			
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
						jsApiList: ['chooseImage',  
									'previewImage',  
									'uploadImage',  
									'downloadImage',] // 必填，需要使用的JS接口列表
					});
					self.$jweixin.ready(function() { //需在用户可能点击分享按钮前就先调用
					});
					self.$jweixin.error(function(res) {
						console.log('error', res)
					});
					self.$Utils.finishFunc('wxJsSdk');
				};
				self.$apis.WxJssdk(postData, callback);
			},
			
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					api.showToast('仅限9张', 'fail');
					return;
				};
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						uni.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						for (var i = 0; i < tempFilePaths.length; i++) {
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								thirdapp_id:2
							}, callback)
						}
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				var callback = function(res){
					if(res.info.data.length > 0){
						self.userData = res.info.data[0];
					}
					self.$Utils.finishFunc('getUserData');
				}
				self.$apis.userGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.userImg{width: 80rpx;height: 80rpx;}
</style>
