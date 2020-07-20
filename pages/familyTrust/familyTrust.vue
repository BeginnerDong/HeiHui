<template>
	<view>
		
		<view class="p-r">
			<input type="text" value="" placeholder="搜索你想找的" class="ss"/>
			<image src="../../static/images/search-icon.png" class="ss-icon p-a"></image>
		</view>
		
		<view class="list flexX">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">投教专区</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">普法课堂</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">金融信托</view>
			<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">普法课堂</view>
			<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">金融信托</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="mx-3 my-3 p-r bB-f5">
			<image src="../../static/images/family-img.png" class="trustImg"></image>
			<view class="font-32 colorf p-1 p-a bottom-0 ">横跨陕甘两省的支党河特大桥顺利合拢</view>
		</view>
		
		<view class="article mx-3 radius10 pb-3 flex1 bB-f5">
			<view class="flex5 flex-1">
				<view class="tit font-32 avoidOverflow2 flex-1">减持猛如虎！大股东突然出手 凶猛套现吓坏科技巨头股价大跌！</view>
				<view class="font-24 color9 pt-2">
					<text class="artSgin">#投教专区</text> 2020-06-03
				</view>
			</view>
			<image src="../../static/images/about-img2.png" class="artImg"></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				liCurr:0
			}
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
.li{padding: 0 30rpx;flex-shrink: 0;}
.trustImg{width: 690rpx;height: 340rpx;}
.artImg{width: 175rpx;height: 140rpx;}
.article .tit{width: 455rpx;}
</style>
