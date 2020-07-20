<template>
	<view class="login font-26 color9">
		
		<image src="../../static/images/the login-img1.png" class="loginBg"></image>
		
		<image src="../../static/images/the login-img.png" class="logo"></image>
		
		<view class="line-h">
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" v-show="is_show==1">
				<image src="../../static/images/the login-icon.png" class="xm-icon"></image>
				<input type="text" value="" placeholder="姓名" />
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2">
				<image src="../../static/images/the login-icon1.png" class="sj-icon"></image>
				<input type="text" value="" placeholder="手机号" />
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2" v-show="is_show==0 || is_show==1">
				<image src="../../static/images/the login-icon2.png" class="mm-icon"></image>
				<input type="text" value="" placeholder="密码" />
			</view>
			<view class="xx flex1 bB-f5 pt-4 pb-2 mb-2"  v-show="is_show==2">
				<image src="../../static/images/the login-icon3.png" class="aq-icon"></image>
				<input type="text" value="" placeholder="验证码" />
				<view class="color2 font-w">获取验证码</view>
			</view>
		</view>
		
		<view class="btn600">确定</view>
		
		<!-- 登录显示 -->
		<view class="flex0 pt-5" v-show="is_show==0">
			<view class="d-flex a-center pr-5 bR-f5" @click="isShow(1)">
				注册账号
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
			<view class="d-flex a-center pl-5" @click="isShow(2)">
			  重置密码
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
		</view>
		<!-- 注册显示 -->
		<view class="flex0 pt-5" v-show="is_show==1">
			<view class="d-flex a-center" @click="isShow(0)">
				已有账号
				<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
			</view>
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
				self.is_show = i;
			}
		}
	}
</script>

<style>
.logo{width: 237rpx;height: 210rpx;margin: 0 auto;margin: 60rpx auto 80rpx;}
.login input{font-size: 26rpx;flex: 1;}
.xx{width: 600rpx;margin: 0 auto;}
.btn600{margin-top: 60rpx;}
.loginBg{width: 100%;height: 242rpx;position: fixed;bottom: 0;}
</style>
