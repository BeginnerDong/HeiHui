<template>
	<view>
		
		<view class="flex3 pt-1">
			<view class="pt-3 p-r newDate">
				<image src="../../static/images/new-icon.png" class="new-icon"></image>
				<view class="colorf p-a top-0 left-0 right-0 w-100 text-center">
					<view class="font-32 pt-4">12</view>
					<view class="font-20">2020/05</view>
				</view>
			</view>
			<view class="px-3">
				<view class="newCon py-3 bB-e1" @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
					<view class="font-32 tit avoidOverflow2">"破局希望"-中国医药行业发展透视与未来展望</view>
					<image src="../../static/images/new-img.png" class="newImg"></image>
				</view>
				<view class="newCon py-3 bB-e1">
					<view class="font-32 tit avoidOverflow2">"破局希望"-中国医药行业发展透视与未来展望</view>
					<image src="../../static/images/new-img.png" class="newImg"></image>
				</view>
			</view>
		</view>
		
		<view class="flex3 pt-1">
			<view class="pt-3 p-r newDate">
				<image src="../../static/images/new-icon.png" class="new-icon"></image>
				<view class="colorf p-a top-0 left-0 right-0 w-100 text-center">
					<view class="font-32 pt-4">12</view>
					<view class="font-20">2020/04</view>
				</view>
			</view>
			<view class="px-3">
				<view class="newCon py-3 bB-e1">
					<view class="font-32 tit avoidOverflow2">"破局希望"-中国医药行业发展透视与未来展望</view>
					<image src="../../static/images/new-img.png" class="newImg"></image>
				</view>
			</view>
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
.newDate{width: 130rpx;}
.newCon .tit{width: 560rpx;}
.newImg{width: 100%;height: 300rpx;margin-top: 20rpx;}
</style>
