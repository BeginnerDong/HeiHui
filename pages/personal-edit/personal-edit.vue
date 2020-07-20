
<template>
	<view class="px-3 bg-white">
		
		<view class="flex1 py-3 bB-e1">
			<view class="color6 flex-1">头像</view>
			<image src="../../static/images/my-img.png" class="userImg"></image>
			<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
		</view>
		<view class="flex1 py-4 bB-e1" @click="Router.navigateTo({route:{path:'/pages/personal-editName/personal-editName'}})">
			<view class="color6 flex-1">昵称</view>
			<view class="font-26">喷火娃</view>
			<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
		</view>
		<view class="flex1 py-4 bB-e1" @click="Router.navigateTo({route:{path:'/pages/personal-editPhone/personal-editPhone'}})">
			<view class="color6 flex-1">电话</view>
			<view class="font-26">15682457856</view>
			<image src="../../static/images/mall-icon3.png" class="R-icon"></image>
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

<style>
page{background-color: #f5f5f5;}
.userImg{width: 80rpx;height: 80rpx;}
</style>
