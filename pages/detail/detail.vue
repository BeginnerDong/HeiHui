<template>
	<view class="px-3">
		
		<view class="font-36 pt-3">{{mainData.title}}</view>
		<view class="font-24 color6 flex1 mt-1 pb-4 bB-e1">
			<view class="flex">
				<image src="../../static/images/details-icon.png" class="time-icon"></image>
				<view>{{mainData.create_time}}</view>
			</view>
			<view class="flex">
				<image src="../../static/images/details-icon2.png" class="sc-icon"></image>
				<view>收藏</view>
			</view>
		</view>
		
		<view class="pt-3">
			<video :src="mainData&&mainData.bannerImg&&mainData.bannerImg[0]&&mainData.bannerImg[0].url" controls></video>
			<view class="pt-3 font-26">
				<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
					
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				searchItem:{
					thirdapp_id: 2
				}
			}
		},
		onLoad(option){
			const self = this;
			self.searchItem.id = option.id;
			self.searchItem.type = option.type;
			self.searchItem.menu_id = option.menu_id;
			console.log('option',option)
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData(isNew,id) {
				const self = this;
				if (isNew) {
					self.topData = [];
					self.mainData = [];
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data[0];
						self.mainData.create_time = self.$Utils.timeto(parseInt(self.mainData.create_time),'ymd');
						console.log('main',self.mainData);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
video{width: 690rpx;height: 360rpx;}
</style>
