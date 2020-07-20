<template>
	<view>
		
		<view class="mx-3 py-4 flex3 bB-f5">
			<view class="pr-5">
				<view class="font-30">张世杰</view>
				<view class="mrSgin">默认</view>
			</view>
			<view class="flex-1 flex1">
				<view class="flex-1">
					<view>159****5867</view>
					<view class="color6 address">陕西省西安市雁塔区科技路高新大都荟A座2007</view>
				</view>
				<image src="../../static/images/address-icon.png" class="add-icon" @click="Router.navigateTo({route:{path:'/pages/shop-addAddress/shop-addAddress'}})"></image>
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
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.add-icon{width: 36rpx;height: 36rpx;}
.mrSgin{line-height: 30rpx;padding: 0 5rpx;border: 1px solid #E39423;font-size: 20rpx;display: inline-block;color: #E39423;border-radius: 4rpx;}
.address{width: 475rpx;}

.addBto{height: 80rpx;position: fixed;left: 0;right: 0;bottom: 0;}
</style>
