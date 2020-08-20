<template>
	<view>
		
		<view class="list flexX p-fXY">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">未使用</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">已使用</view>
			<!-- <view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已过期</view> -->
		</view>
		
		<view class="pb-3 card">
			<view class="mx-3 mt-3 colorf p-r" v-for="(item,index) in mainData" :key="index">
				<image src="../../static/images/coupons-img1.png" v-if="item.transport_status==0" class="cardImg"></image>
				<image src="../../static/images/coupons-img.png" v-if="item.transport_status==2" class="cardImg"></image>
				<view class="p-aXY flex1 px-3 h-100 line-h">
					<view >
						<view class="font-40 pb-5">{{item.child&&item.child[0]?item.child[0].title:''}}</view>
						<view class="font-30 pb-2">仅限兑换上述指定商品使用</view>
						<!-- <view class="font-22">有效期：2019.02.20-2019.11.30</view> -->
					</view>
					<view class="colorM font-w pr-2 font-30" v-if="item.transport_status==0" @click="orderUpdate(index)">点击使用</view>
					<view class="font-w pr-2 font-30 colorG" v-if="item.transport_status==2">已经使用</view>
					<!-- <view class="font-w pr-2 font-30 colorG">已过期</view> -->
				</view>
			</view>
		</view>
		<view style="width: 100%;text-align: center;" v-if="mainData.length==0">暂无数据~</view>
		
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				liCurr:0,
				searchItem: {
					level: 1,
					pay_status: 1,
					type: 2,
					transport_status:0
				},
				mainData:[]
			}
		},
		
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
							self.changeLi(1)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			changeLi(i){
				const self = this;
				if(self.liCurr!=i){
					self.liCurr = i;
					if(self.liCurr==0){
						self.searchItem.transport_status = 0
					}else if(self.liCurr==1){
						self.searchItem.transport_status = 2
					};
					self.getMainData(true)
				}	
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
		}
	}
</script>

<style scoped>
.list{height: 100rpx;z-index: 100;background-color: #fff;}
.li{width: 50%;}
.card{margin-top: 130rpx;}
.cardImg{width: 693rpx;height: 226rpx;}
.colorG{color: #B3B6BB;}
</style>
