<template>
	<view>
		
		<video src="" controls></video>
		
		<view class="list flexX shadow">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">简介</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">演讲人</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">总结</view>
		</view>
		
		<view class="px-3">
			<view v-show="liCurr==0">
				<view class="pt-4 pb-2">他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业（有限合伙）、天津当科企业管理咨询合伙企业（有限合伙）等担任法人，他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业</view>
			</view>
			<view v-show="liCurr==1">
				<view class="pt-4 pb-2">主讲人：宋彩亚</view>
				<view class="color6 font-26">
					他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业（有限合伙）、天津当科企业管理咨询合伙企业（有限合伙）等担任法人，他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业
				</view>
				<view class="pt-5 pb-2">对讲嘉宾：宋彩亚</view>
				<view class="color6 font-26">
					他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业（有限合伙）、天津当科企业管理咨询合伙企业（有限合伙）等担任法人，他(她)在北京国创庆业管理咨询有限公司、天津国略企业管理咨询合伙企业
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
video{width: 100%;height: 480rpx;}
.li{width: 33.33%;}
</style>
