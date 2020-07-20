<template>
	<view>
		
		<view class="flex3 pt-1">
			<view class="pt-3 p-r font-26 pl-3 flex-1 date">
				<text class="font-42">08</text>2月
			</view>
			<view class="px-3 mt-3 bL-f5">
				<view class="acvCon pb-3">
					<view class="font-32 avoidOverflow2 tit">"破局希望"-中国医药行业发展透视与未来展望</view>
					<image src="../../static/images/new-img.png" class="acvImg"></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.acvCon .tit{width: 530rpx;}
.acvImg{width: 100%;height: 300rpx;margin-top: 20rpx;}
.date::before{content: ''; width: 14rpx;height: 14rpx;position: absolute;top: 40%;right: -8rpx; background-color: #E39423;border-radius: 50%;}
</style>
