<template>
	<view>
		
		<!-- <image src="../../static/images/jianjie-img.png" class="img"></image> -->
		
		<image :src="mainData&&mainData.mainImg&&mainData.mainImg[0]&&mainData.mainImg[0].url" mode="widthFix"></image>
		
		<!-- <view class="p-r item">
			<image src="../../static/images/jianjie-img4.png" class="bg-txt"></image>
			<view class="flex0 pt-5 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">公司简介</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 txt">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
					
				</view>
			</view>
		</view> -->
		
		
		<!-- <view class="p-r item">
			<view class="flex0 pt-3 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">企业文化</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 txt">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.passage1">
					
				</view>
				<image src="../../static/images/jianjie-img1.png" class="img1"></image>
			</view>
		</view> -->
		
		<!-- <view class="p-r item">
			<view class="flex0 pt-3 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">合作伙伴</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 flex flex-wrap">
				<image v-for="(item,index) in mainData.bannerImg" :src="item.url" class="img2"></image>
			</view>
		</view> -->
		
		<view class="p-r item">
			<view class="flex0 pt-3 z-index10 p-r">
				<image src="../../static/images/jianjie-icon.png" class="jj-icon"></image>
				<view class="color2 font-44 font-w text-center px-2">联系我们</view>
				<image src="../../static/images/jianjie-icon1.png" class="jj-icon"></image>
			</view>
			<view class="mx-3 p-3 mt-3 p-r shadowM radius10 contact">
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
		
		<image src="../../static/images/jianjie-img2.png" class="img3"></image>
		
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
				}
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/introduction/introduction')
			self.$Utils.loadAll(['getMainData','getUserData'], self);
		},
		
		methods: {
			
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
						    title: '提交成功',
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
			
			getMainData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					menu_id: 1,
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
