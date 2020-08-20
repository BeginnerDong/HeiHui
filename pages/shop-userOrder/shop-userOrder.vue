<template>
	<view>
		
		<view class="list flexX p-fXY bg-white shadow">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待发货</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">待收货</view>
			<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">已完成</view>
			<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">已退款</view>
		</view>
		
		<view style="height: 120rpx;"></view>
		<view class="bg-white mx-3 radius10 p-2 mb-2 oredrBox"  v-for="(item,index) in mainData" :key="index">
			<view class="flex1 pb-3 font-24">
				<view class="flex">
					<image src="../../static/images/my order-icon.png" class="mai-icon"></image>
					<view>{{item.create_time}}</view>
				</view>
				<view class="colorR" v-if="item.pay_status==1&&item.transport_status==0&&item.order_step==0">待发货</view>
				<view class="colorR" v-if="item.pay_status==1&&item.transport_status==1&&item.order_step==0">已发货</view>
				<view class="colorR" v-if="item.pay_status==1&&item.transport_status==2&&item.order_step==0">已完成</view>
				<view class="colorR" v-if="item.order_step==1">已申请退款</view>
				<view class="colorR" v-if="item.order_step==2">已退款</view>
			</view>
			<!-- 全部、待发货、待收货、已完成展示 -->
			<view>
				<view class="d-flex a-start j-sb pt-3 bT-f5 order" v-for="(c_item,c_index) in item.child" :key="c_index" 
				:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/shop-orderDetail/shop-orderDetail?id='+$event.currentTarget.dataset.id}})">
					<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" class="myorederImg"></image>
					<view class="myCon pl-2 flex5">
						<view class="tit avoidOverflow2 pb-1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product?c_item.orderItem[0].snap_product.product.title:''}}</view>
						<view class="font-22 color6 line-h flex-1">规格：
						{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
						<view class="font-24" v-if="item.order_step>0">退款金额: <text class="font-32 colorR">¥78.8</text></view>
					</view>
					<view>
						<view class="price1">{{c_item.unit_price}}</view>
						<view class="font-22 color9 text-right pt-1">x{{c_item.count}}</view>
					</view>
				</view>
				<view class="font-24 color9 pt-3" v-if="item.order_step>0">
					<view>收件人：<text class="font-28 color2 px-1">{{item.snap_address?item.snap_address.name:''}}</text> {{item.snap_address?item.snap_address.phone:''}}</view>
					<view>申请时间：{{item.update_time}}</view>
				</view>
				<view class="color9 text-right pb-3">实付款：<text class="price1 font-w">{{item.price}}</text></view>
			</view>
			
			<view class="d-flex j-end">
				<view class="tkBtn b-e1 font-24" v-if="item.transport_status==0&&item.order_step==0&&item.pay_status==1"
					:data-id="item.id" 
					@click="Router.navigateTo({route:{path:'/pages/shop-refund/shop-refund?id='+$event.currentTarget.dataset.id}})">申请退款</view>
				<view class="tkBtn borderM font-24 colorM" @click="orderUpdate(index)" v-if="item.transport_status==1&&item.order_step==0&&item.pay_status==1">确认收货</view>
			</view>
			
			<!-- 已退款展示 -->
			<!-- <view>
				<view class="d-flex a-start j-sb pt-3 bT-f5 order">
					<image src="../../static/images/my order-img.png" class="myorederImg"></image>
					<view class="myTkCon pl-2 d-flex flex-column">
						<view class="tit avoidOverflow2 pb-1">哼唱幸福 11枝粉色扶郎花束5555555555555555</view>
						<view class="font-22 color6 line-h flex-1">规格：礼盒A</view>
						<view class="font-24">退款金额: <text class="price1 font-32">78.8</text></view>
					</view>
				</view>
				<view class="font-24 color9 pt-3">
					<view>收件人：<text class="font-28 color2 px-1">张萌</text> 15665988946</view>
					<view>申请时间：2020-6-29 10:20:24</view>
				</view>
			</view> -->
			
		</view>
		
		<view style="width: 100%;text-align: center;" v-if="mainData.length==0">暂无数据相关数据~</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:-1,
				searchItem: {
					level: 1,
					pay_status: 1,
					type: 1
				},
				mainData: [],
			}
		},
		
		
		
		onLoad(options) {
			const self = this;
			if(options.id){
				self.changeLi(options.id)
			}else{
				self.liCurr = 0;
				self.$Utils.loadAll(['getMainData'], self);
			}
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
		},
		
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			
			orderUpdate(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			changeLi(i){
				const self = this;
				if(self.liCurr != i){
					self.liCurr = i;
					if(self.liCurr==0){
						delete self.searchItem.transport_status;
						delete self.searchItem.order_step;
					}else if(self.liCurr==1){
						self.searchItem.transport_status = 0;
						self.searchItem.order_step = 0
					}else if(self.liCurr==2){
						self.searchItem.transport_status = 1;
						self.searchItem.order_step = 0
					}else if(self.liCurr==3){
						self.searchItem.transport_status = 2;
						self.searchItem.order_step = 0
					}else if(self.liCurr==4){
						self.searchItem.order_step > 0
					}
					self.getMainData(true)
				}
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.list{height: 100rpx;z-index: 100;}
.li{width: 20%;}
.mai-icon{width: 32rpx;height: 32rpx;margin-right: 10rpx;}
.myorederImg{width: 170rpx;height: 170rpx;}
.myCon .tit{width: 334rpx;}
.myCon .price1{color: #222;font-size: 32rpx;}
.tkBtn{line-height: 60rpx;width: 150rpx;text-align: center;border-radius: 30rpx;}
.myTkCon .tit{width: 465rpx;}
.myTkCon{height: 170rpx;}
.myCon{height:170rpx}
</style>
