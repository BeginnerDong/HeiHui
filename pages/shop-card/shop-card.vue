<template>
	<view>
		
		<view class="list flexX p-fXY">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">未使用</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">已使用</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已过期</view>
		</view>
		
		<view class="pb-3 card">
			<view class="mx-3 mt-3 colorf p-r">
				<image src="../../static/images/coupons-img1.png" class="cardImg"></image>
				<!-- <image src="../../static/images/coupons-img.png" class="cardImg"></image> -->
				<view class="p-aXY flex1 px-3 h-100 line-h">
					<view >
						<view class="font-40 pb-5">满400元减200元</view>
						<view class="font-30 pb-2">仅限指定商品使用</view>
						<view class="font-22">有效期：2019.02.20-2019.11.30</view>
					</view>
					<view class="colorM font-w pr-2 font-30">点击使用</view>
					<!-- <view class="font-w pr-2 font-30 colorG">已经使用</view> -->
					<!-- <view class="font-w pr-2 font-30 colorG">已过期</view> -->
				</view>
			</view>
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
.list{height: 100rpx;z-index: 100;background-color: #fff;}
.li{width: 33.33%;}
.card{margin-top: 130rpx;}
.cardImg{width: 693rpx;height: 226rpx;}
.colorG{color: #B3B6BB;}
</style>
