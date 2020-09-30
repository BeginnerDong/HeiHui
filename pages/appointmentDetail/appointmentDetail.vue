<template>
	<view>
		
		<video :src="mainData&&mainData.bannerImg&&mainData.bannerImg[0]&&mainData.bannerImg[0].url" controls
		v-show="mainData&&mainData.bannerImg&&mainData.bannerImg[0]&&mainData.bannerImg[0].url"></video>
		
		<view class="list flexX shadow">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">简介</view>
			<!-- <view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">演讲人</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">总结</view> -->
		</view>
		
		<view class="p-3">
			<view v-show="liCurr==0">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.passage1">
					
				</view>
			</view>
			<view v-show="liCurr==1">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
					
				</view>
			</view>
			<view v-show="liCurr==2">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.passage1">
					
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				liCurr:0,
				mainData:{}
			}
		},
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('appointmentData');
		},
		methods: {
			
			changeLi(i){
				const self = this;
				self.liCurr = i;
			}
			
		}
	}
</script>

<style scoped>
video{width: 100%;height: 480rpx;}
.li{width: 33.33%;}
</style>
