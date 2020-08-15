<template>
	<view class="px-3">
		
		<view class="map my-2 shadowM">
			<image src="../../static/images/contactus-img.png" mode=""></image>
		</view>
		
		<view class="flex1 py-3 pl-4 shadowM line-h mb-2">
			<image src="../../static/images/contactus-icon.png" class="us-icon"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">联系电话</view>
				<view>{{mainData.phone}}</view>
			</view>
		</view>
		<view class="flex1 py-3 pl-4 shadowM line-h mb-2">
			<image src="../../static/images/contactus-icon1.png" class="us-icon"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">联系地址</view>
				<view>{{mainData.description}}</view>
			</view>
		</view>
		<view class="flex3 py-3 pl-4 shadowM line-h mb-2">
			<image src="../../static/images/contactus-icon2.png" class="us-icon1"></image>
			<view class="flex-1 pl-3">
				<view class="font-24 color6 pb-2">意见反馈</view>
				<textarea v-model="data.content" placeholder="请输入" />
				<view class="btn540 mt-2" @click="submit">提交</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				data:{
					title:'',
					phone:'',
					content:''
				},
				userData:{}
			}
		},
		onLoad(){
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			submit(){
				const self = this;
				// self.data.title = 
				// self.data.phone = 
				const postData = {
					data:self.data
				};
				postData.tokenFuncName = 'getProjectToken';
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
						self.$Utils.showToast('网络故障', 'none')
					}
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getProjectToken';
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						
					}
					console.log('userData',self.userData)
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
			
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
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
.map{height: 300rpx;width: 690rpx;}
.map image{width: 100%;height: 100%;}
textarea{width: 540rpx;height: 240rpx;background-color: #f5f5f5;font-size: 22rpx;padding: 20rpx;box-sizing: border-box;margin: 0;}
</style>
