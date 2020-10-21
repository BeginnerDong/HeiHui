<template>
	<view>
		
		<view class="top px-3 py-4 Mgb flex font-36 colorf font-w">
			<image src="../../static/images/the order details-icon.png" class="order-icon1" 
			v-if="mainData.pay_status==1&&mainData.transport_status==0&&mainData.order_step==0"></image>
			<image src="../../static/images/the order details-icon1.png" class="order-icon1"
			v-if="mainData.pay_status==1&&mainData.transport_status==1&&mainData.order_step==0"></image>
			<image src="../../static/images/the order details-icon2.png" class="order-icon1"
			v-if="mainData.pay_status==1&&mainData.transport_status==2&&mainData.order_step==0"></image>
			<image src="../../static/images/the order details-icon3.png" class="order-icon2" v-if="mainData.order_step>0"></image>
			<view v-if="mainData.pay_status==1&&mainData.transport_status==0&&mainData.order_step==0">待发货</view>
			<view v-if="mainData.pay_status==1&&mainData.transport_status==1&&mainData.order_step==0">已发货</view>
			<view v-if="mainData.pay_status==1&&mainData.transport_status==2&&mainData.order_step==0">已完成</view>
			<view  v-if="mainData.order_step==1">已申请退款</view>
			<view  v-if="mainData.order_step==2">已退款</view>
		</view>
		
		<view class="d-flex p-3 line-h bg-white mb-2">
			<image src="../../static/images/the order details-icon4.png"  class="order-icon3"></image>
			<view>
				<view class="font-w pb-3"><text class="pr-3">{{mainData.snap_address?mainData.snap_address.name:''}}</text>{{mainData.snap_address?mainData.snap_address.phone:''}}</view>
				<view class="color6">{{mainData.snap_address?mainData.snap_address.city+mainData.snap_address.detail:''}}</view>
			</view>
		</view>
		
		<view class="px-3 bg-white mb-2">
			<view class="d-flex a-start j-sb py-3 bB-f5" v-for="(c_item,c_index) in mainData.child" :key="c_index">
				<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" class="myorederImg"></image>
				<view class="myCon pl-2 flex-1">
					<view class="tit avoidOverflow2"  style="margin-bottom: 33px;">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product?c_item.orderItem[0].snap_product.product.title:''}}</view>
					<view class="font-22 color6 line-h flex-1">
						<span v-for="(cc_item,key,cc_index) in c_item.orderItem[0].snap_product.label_description" :key="key">{{key}}:{{cc_item}}</span>
					</view>
				</view>
				<view>
					<view class="price1">{{c_item.unit_price}}</view>
					<view class="font-22 color9 text-right pt-1">x{{c_item.count}}</view>
				</view>
			</view>
		</view>
		
		<view class="bg-white px-3 mb-2">
			<view class="flex1 py-3 bB-f5">
				<view>商品总额</view>
				<view class="price1 font-w">{{mainData.price?mainData.price:''}}</view>
			</view>
			<view class="text-right py-3 sfk">实付款：<text class="price1 font-w font-36">{{mainData.price?mainData.price:''}}</text></view>
		</view>
		
		<view class="bg-white p-3 font-24 color6 line-h" v-if="mainData.order_step>0">
			<view class="pb-3">退款原因：{{mainData.passage1}}</view>
			<view class="pb-3">订单编号：{{mainData.order_no}}</view>
			<view class="pb-3">下单时间：{{mainData.create_time}}</view>
			<!-- <view>付款类型：微信付款</view> -->
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
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
	}
</script>

<style>
page{background-color: #f5f5f5;}
.order-icon1{width: 78rpx;height: 70rpx;margin-right: 30rpx;}
.order-icon2{width: 70rpx;height: 70rpx;margin-right: 30rpx;}
.order-icon3{width: 38rpx;height: 38rpx;margin-right: 20rpx;}
.myorederImg{width: 173rpx;height: 173rpx;}
.price1{color: #222;}
.myCon .tit{width: 335rpx;}
.sfk .price1{color: #E80F0F;}
</style>
