<template>
	<view class="px-3">
		
		<view class="font-36 pt-3">{{mainData.title}}</view>
		<view class="font-24 color6 flex1 mt-1 pb-4 bB-e1">
			<view class="flex">
				<image src="../../static/images/details-icon.png" class="time-icon"></image>
				<view>{{mainData.create_time}}</view>
			</view>
			<view class="flex" @click="Utils.stopMultiClick(collect)">
				<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/details-icon2.png':'../../static/images/details-icon1.png'" class="sc-icon"></image>
				<view>{{mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'已收藏':'收藏'}}</view>
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
				},
				Utils:this.$Utils
			}
		},
		onLoad(option){
			const self = this;
			var options = self.$Utils.getHashParameters();
			console.log(options);
			self.searchItem.type = options[0].type;
			self.searchItem.id = options[0].id;
			//self.searchItem.menu_id = options[0].menu_id;
			console.log('option',option)
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if(self.userInfoData.behavior==1){
							self.isVip = true
						};
						self.$Utils.finishFunc('getUserInfoData');
					}
				}
				self.$apis.userInfoGet(postData, callback);
			},
			
			collect() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(!self.isVip){
					self.$Utils.showToast('抱歉，非会员不可收藏', 'none', 1000)
					uni.setStorageSync('canClick', true);
					return
				};
				if (self.mainData.log.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '收藏成功',
					relation_id: self.mainData.id,
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				if(self.searchItem.type==2){
					postData.data.behavior=2
				}else{
					postData.data.behavior=1
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.log.push({
							status: 1,
							id: res.info.id
						});
					} else {
						self.$Utils.showToast('收藏失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData.log[0].id
					},
					data: {
						status: -self.mainData.log[0].status
					}
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.log[0].status = -self.mainData.log[0].status;
			
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					log:{
						token:uni.getStorageSync('user_token'),
						
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				if(self.searchItem.type==2){
					postData.getAfter.log.searchItem.behavior=2
				}else{
					postData.getAfter.log.searchItem.behavior=1
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data[0];
						self.mainData.create_time = self.mainData.create_time.substr(0,10);
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
