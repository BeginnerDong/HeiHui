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
		
		<view class="article mt-3 mx-3 shadowM radius10 px-2 py-3 flex1" @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
			<view>
				<view class="tit font-30 avoidOverflow3">减持猛如虎！大股东突然出手 凶猛套现吓坏科技巨头股交电三级导航费房河南</view>
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
				Router:this.$Router,
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
</style>
