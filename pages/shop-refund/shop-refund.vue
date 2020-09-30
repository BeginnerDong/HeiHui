<template>
	<view>
		
		<view class="d-flex a-start p-3 bT-f5 bg-white mb-2" v-for="(item,index) in mainData.child" :key="index">
			<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product&&
			item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?item.orderItem[0].snap_product.product.mainImg[0].url:''" class="myorederImg"></image>
			<view class="myTkCon pl-2 d-flex flex-column">
				<view class="tit avoidOverflow2 pb-1">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.title:''}}</view>
				<view class="font-22 color6 line-h flex-1">规格：{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
			</view>
		</view>
		
		<view class="d-flex bg-white p-3 mb-2">
			<view>退款原因：</view>
			<textarea value="" placeholder="请说明" v-model="submitData.passage1"/>
		</view>
		
		<view class="bg-white p-3">退款金额: <text class="price1 font-32 font-w">{{mainData.price}}</text></view>
		
		<view class="submit Mgb colorf font-w p-f" @click="orderUpdate()">提交</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				submitData:{
					passage1:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		

		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.passage1==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入退款原因','none');
					return
				}
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					order_step:1,
					passage1:self.submitData.passage1
				};
				postData.searchItem = {
					id:self.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.Router.redirectTo({route:{path:'/pages/shop-userOrder/shop-userOrder'}})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					id: self.id,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.myorederImg{width: 170rpx;height: 170rpx;}
.myTkCon .tit{width: 465rpx;}
.myTkCon{height: 170rpx;}
textarea{width: 540rpx;height: 120rpx;font-size: 28rpx;}
.submit{line-height: 100rpx;height: 100rpx;text-align: center;left: 0;right: 0;bottom: 0;}
</style>
