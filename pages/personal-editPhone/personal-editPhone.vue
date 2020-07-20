<template>
	<view>
		
		<view v-show="is_show==0">
			<image src="../../static/images/replace the phone-icon1.png" class="phone-icon1"></image>
			<view class="text-center">绑定手机号：15684792385</view>
			<view class="btn600" @click="isShow(1)">更换手机号</view>
		</view>
		
		<view v-show="is_show==1">
			<image src="../../static/images/replace the phone-icon.png" class="phone-icon"></image>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2">
				<input type="text" value="" placeholder="密码" />
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" >
				<input type="text" value="" placeholder="验证码" />
				<view class="colorM font-w">获取验证码</view>
			</view>
			<view class="btn600" @click="isShow(0)">完成</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				is_show:0
			}
		},
		methods: {
			isShow(i){
				const self = this;
				self.is_show = i
			}
		}
	}
</script>

<style scoped>
.phone-icon{height: 222rpx;width: 138rpx;margin: 100rpx auto 70rpx;}
.phone-icon1{height: 222rpx;width: 174rpx;margin: 100rpx auto 70rpx;}
.btn600{margin-top: 100rpx;}

input{font-size: 26rpx;flex: 1;}
.xx{width: 600rpx;margin: 0 auto;}
</style>
