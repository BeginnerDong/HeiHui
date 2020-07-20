<template>
	<view>
		<view>
			<image src="../../static/images/about-img.png" class="appoinImg"></image>
		</view>
		
		<view class="flex py-2 px-3">
			<image src="../../static/images/about-icon.png" class="app-icon"></image>
			<view class="font-32 font-w">视频预告</view>
		</view>
		<view class="flex px-3">
			<image src="../../static/images/family-img1.png" class="spImg"></image>
			<view class="line-h pl-2 flex-1 spTxt">
				<view class="pb-4 avoidOverflow">主题：艺术珠宝鉴赏与投资</view>
				<view class="pb-4">时间：2020-07-03 20:00</view>
				<view>专家：宋彩亚</view>
			</view>
		</view>
		<view class="font-24 px-3">
			<view class="pt-3 pb-2">主讲人：宋彩亚</view>
			<view class="color6">
				他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业（有限合伙）、天津当科企业管理咨询合伙企业（有限合伙）等担任法人，他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业
			</view>
			<view class="pt-3 pb-2">对讲嘉宾：宋彩亚</view>
			<view class="color6">
				他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业（有限合伙）、天津当科企业管理咨询合伙企业（有限合伙）等担任法人，他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业
			</view>
		</view>
		
		<view class="flex py-2 px-3">
			<image src="../../static/images/about-icon.png" class="app-icon"></image>
			<view class="font-32 font-w">往期视频回放</view>
		</view>
		<view class="font-24 line-h flex2 pb-4">
			<view class="flex4" @click="changeLi(0)">
				<image src="../../static/images/baike-icon3.png" class="app-icon1" v-show="liCurr!=0"></image>
				<image src="../../static/images/baike-icon4.png" class="app-icon1" v-show="liCurr==0"></image>
				<view>资产配置</view>
			</view>
			<view class="flex4" @click="changeLi(1)">
				<image src="../../static/images/baike-icon2.png" class="app-icon1" v-show="liCurr!=1"></image>
				<image src="../../static/images/baike-icon5.png" class="app-icon1" v-show="liCurr==1"></image>
				<view>家族财富管理</view>
			</view>
			<view class="flex4" @click="changeLi(2)">
				<image src="../../static/images/baike-icon1.png" class="app-icon1" v-show="liCurr!=2"></image>
				<image src="../../static/images/baike-icon6.png" class="app-icon1" v-show="liCurr==2"></image>
				<view>其他</view>
			</view>
		</view>
		
		<view class="flex1 mx-3 pb-3 p-r" @click="isShow">
			<image src="../../static/images/about-img1.png" class="spImg"></image>
			<view class="pl-2 wqTxt">
				<view class="font-30 font-w avoidOverflow2">中国医药行业发展透视与未来与发展</view>
				<view class="font-26 pt-3 avoidOverflow2">简介：他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业（有限合伙）、天津当科企业管理咨询合伙企业（有限合伙）等</view>
			</view>
			<view class="font-22 p-1 Mgb colorf line-h p-a top-0 left-0">专家 | 唐心</view>
		</view>
		
		
		<!-- 弹框 -->
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white mx-3 radius10 text-center font-30 colorf p-r tcBox">
				<image src="../../static/images/x-icon.png" class="x-icon" @click="isShow"></image>
				<view class="Mgb tit">请输入密码</view>
				<view class="py-5">
					<input type="text" value="" placeholder="姓名" />
					<input type="text" value="" placeholder="密码" />
					<view class="btn600" @click="Router.navigateTo({route:{path:'/pages/appointmentDetail/appointmentDetail'}})">进入</view>
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

<style scoped>
.appoinImg{width: 100%;height: 400rpx;}
.app-icon{width: 30rpx;height: 31rpx;margin-right: 10rpx;}
.app-icon1{width: 110rpx;height: 110rpx;margin-bottom: 30rpx;}
.spImg{width: 260rpx;height: 180rpx;}
.spTxt view{width: 410rpx;}
.wqTxt view{width: 408rpx;}

.tcBox{margin-top: 40%;overflow: hidden;}
.tcBox .tit{line-height: 80rpx;}
.tcBox input{width: 600rpx;height: 80rpx;font-size: 26rpx;margin: 0 auto 30rpx;border: 1px solid #e1e1e1;border-radius: 10rpx;}
.btn600{line-height: 70rpx;}
</style>
