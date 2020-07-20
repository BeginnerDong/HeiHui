<template>
	<view>
		
		<view class="p-r colorf">
			<image src="../../static/images/personal center-img.png" class="perBg"></image>
			<view class="flex1 h-100 p-a top-0 px-3">
				<image src="../../static/images/my-img.png" class="userImg"></image>
				<view class="flex-1 font-32 pl-2">哆啦A梦</view>
			</view>
			<view class="p-a top-0 flex4 right-0 m-3" @click="Router.navigateTo({route:{path:'/pages/personal-edit/personal-edit'}})">
				<image src="../../static/images/personal center-icon.png" class="per-icon"></image>
				<view class="font-24 pt-1">编辑</view>
			</view>
		</view>
		
		<view class="list">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">收藏·文章新闻</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">收藏·热销产品</view>
		</view>
		
		<view v-show="liCurr==0">
			<view class="article mt-3 mx-3 shadowM radius10 px-2 py-3 flex1">
				<view>
					<view class="tit font-30 avoidOverflow3">减持猛如虎！大股东突然出手 凶猛套现吓坏科技巨头股交电三级导航费房河南</view>
					<view class="font-24 color9 pt-2">
						<text class="artSgin">#投教专区</text> 2020-06-03
					</view>
				</view>
				<image src="../../static/images/about-img2.png" class="artImg"></image>
			</view>
			<view class="article mt-3 mx-3 shadowM radius10 px-2 py-3 flex1">
				<view>
					<view class="tit font-30 avoidOverflow3">减持猛如虎！大股东突然出手 凶猛套现吓坏科技巨头股交电三级导航费房河南</view>
					<view class="font-24 color9 pt-2">
						<text class="artSgin">#投教专区</text> 2020-06-03
					</view>
				</view>
				<image src="../../static/images/about-img2.png" class="artImg"></image>
			</view>
		</view>
		
		<view v-show="liCurr==1">
			<view class="hot m-3 shadowM radius10 overflow-h font-24 p-r bg-white" @click="isShow">
				<image src="../../static/images/product-icon11.png" class="hotBg"></image>
				<view class="colorf d-flex px-2 p-r py-4 borderDB">
					<view class="font-22 hotSgin">热门</view>
					<view class="font-32 color2 pl-1 tit">恒辉-嘉润7号集合资金信托计划（第3期F类）</view>
					<view class="qySgin">工商企业</view>
				</view>
				<view class="flex3 pt-5 px-2 pb-4">
					<view class="flex4">
						<view class="flex1 pb-1">
							<image src="../../static/images/product-icon5.png" class="yq-icon"></image>
							<view>预期收益最高可达</view>
						</view>
						<view class="colorR font-50 font-w">8.4%</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon6.png" class="yq-icon1"></image>
							<view>项目期限(月)</view>
						</view>
						<view class="font-40">28</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon7.png" class="yq-icon1"></image>
							<view>起投金额(万)</view>
						</view>
						<view class="font-40">100</view>
					</view>
				</view>
			</view>
		</view>
		
		
		
		
		<!-- 重要提示 -->
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-r overflow-h tips">
				<image src="../../static/images/product-icon8.png" class="x-icon" @click="isShow"></image>
				<view class="flex0 font-30 font-w pb-5 pt-3 mx-3">
					<image src="../../static/images/product-icon9.png" class="tips-icon"></image>
					<view>重要提示</view>
				</view>
				<view class="font-26 pt-2 mx-3">
					本人承诺符合中国银行保险监督管理委员会关于“合格投资者”认定的条件。<br /><br />
					本人承诺符合中国银行保险监督管理委员会关于“合格投资者”认定的条件。
					本人承诺符合中国银行保险监督管理委员会关于“合格投资者”认定的条件。
					本人承诺符合中国银行保险监督管理委员会关于“合格投资者”认定的条件。<br /><br />
					本人承诺符合中国银行保险监督管理委员会关于“合格投资者”认定的条件。
				</view>
				<view class="p-a bottom-0 left-0 right-0">
					<view class="colorf font-24 pb-1">
						<image src="../../static/images/product-icon10.png" class="tips-icon1"></image>
						<view class="time">10秒倒计时</view>
					</view>
					<view class="tipBtn">确定</view>
				</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0,
				is_show:false
			}
		},
		methods: {
			changeLi(i){
				const self = this;
				self.liCurr = i;
			},
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			}
		}
	}
</script>

<style>
.perBg{width: 750rpx;height: 200rpx;}

.li{width: 50%;}

</style>
