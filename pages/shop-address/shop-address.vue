<template>
	<view>
		
		<view class="mx-3 py-4 flex3 bB-f5" v-for="(item,index) in mainData" :key="index">
			<view class="pr-5">
				<view class="font-30">{{item.name}}</view>
				<view class="mrSgin" v-if="item.isdefault==1">默认</view>
			</view>
			<view class="flex-1 flex1">
				<view class="flex-1">
					<view>{{item.phone}}</view>
					<view class="color6 address">{{item.city+item.detail}}</view>
				</view>
				<image src="../../static/images/address-icon.png" class="add-icon"
				 :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/shop-addAddress/shop-addAddress?id='+$event.currentTarget.dataset.id}})"></image>
			</view>
		</view>
		
		<view class="addBto text-center flex0 colorM line-h bT-f5" @click="Router.navigateTo({route:{path:'/pages/shop-addAddress/shop-addAddress'}})">
			<view class="font-50 pr-1">+</view>
			<view>新增地址</view>
		</view>
		
	</view>
</template>

<script>
	export default {

		data() {
			return {
				mainData: [],
				Router: this.$Router,
				choosedIndex: -1
			}
		},

		onShow(options) {
			const self = this;
			self.mainData = [];
			
			self.$Utils.loadAll(['getMainData'], self)
		
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},

		methods: {

			choose(index) {
				const self = this;
				self.choosedIndex = index;
				uni.setStorageSync('choosedAddressData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
				uni.navigateBack({
					delta: 1
				})
			},

			getMainData() {
				const self = this;

				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},





			deleteAddress(id) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认是否删除这个地址',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.searchItem = {};
							postData.searchItem.id = id;
							postData.tokenFuncName = 'getUserToken';
							const callback = (res) => {
								if (res) {
									self.mainData = [];
									self.getMainData();
								}
							};
							self.$apis.addressDelete(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},


			updateAddress(id) {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getUserToken';

				postData.searchItem = {};
				postData.searchItem.id = id;
				postData.data = {
					isdefault: 1
				}
				const callback = (res) => {
					if (res) {
						self.mainData = [];
						self.getMainData();
					}
				};
				self.$apis.addressUpdate(postData, callback);
			},




		}
	}
</script>

<style scoped>
.add-icon{width: 36rpx;height: 36rpx;}
.mrSgin{line-height: 30rpx;padding: 0 5rpx;border: 1px solid #E39423;font-size: 20rpx;display: inline-block;color: #E39423;border-radius: 4rpx;}
.address{width: 475rpx;}

.addBto{height: 80rpx;position: fixed;left: 0;right: 0;bottom: 0;}
</style>
