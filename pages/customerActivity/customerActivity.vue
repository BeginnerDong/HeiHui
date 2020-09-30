<template>
	<view>
		
		<view class="flex3 pt-1" v-for="(item,index) in mainData" :key="index">

			<view class="pt-3 p-r newDate">
				<image src="../../static/images/new-icon.png" class="new-icon"></image>
				<view class="colorf p-a top-0 left-0 right-0 w-100 text-center">
					<view class="font-32 pt-4">{{Utils.substr(item.menu,0,4)}}</view>
					<view class="font-20">{{Utils.substr(item.menu,5,10)}}</view>
				</view>
			</view>
			<view class="px-3">
				
				<block v-for="(c_item,c_index) in item.data" :key="c_index">
					<view class="acvCon  py-3 bB-e1"
					@click="toDetail(index,c_index)">
						<view class="font-34 tit avoidOverflow2" style="font-weight: 700;">{{c_item.title}}</view>
						<image :src="c_item.mainImg[0].url" class="acvImg"></image>
					</view>
				</block>
			</view>
		</view>
		
		<view class="bg-mask" v-show="is_show1">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">
					<view class="content ql-editor text-center" style="padding:0;" v-html="tipsData.content">
					</view>
				</view>
				<view class="" style="margin-bottom: 30rpx;color: #888;">联系客服：{{kefuPhone}}</view>
				<view class="flex1 bT-f5 btn1">
					<view class="w-50 bR-f5" @click="showToast1">我知道了</view>
					<view class="colorM w-50" @click="phoneCall()">联系客服</view>
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
				Utils:this.$Utils,
				timeData:[],
				mainData:[],
				is_show1: false,
				tipsData: {},
				kefuData: {},
				kefuPhone:''
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/customerActivity/customerActivity')
			self.$Utils.loadAll(['getMainData','getTipsData','getKefuData'], self);
		},
		onShow() {
			const self = this;
			self.getUserInfoData();
		},
		methods: {
			
			toDetail(index,c_index){
				const self = this;
				if(!self.isVip){
					self.is_show1 = true;
					return
				}
				self.Router.navigateTo({route:{path:'/pages/detail/detail?menu_id=7&id='+self.mainData[index].data[c_index].id}})
			},
			
			getKefuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id: 3,
					thirdapp_id: 2
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.kefuData = res.info.data[0];
					}
					self.$Utils.finishFunc('getKefuData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.getAfter = {
					staff:{
						tableName:'User',
						middleKey:'staff_no',
						key:'staff_no',
						searchItem:{
							status:1,
							user_type:1
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0].info;
						if(self.userInfoData.behavior==1||self.userInfoData.user_type==1){
							self.isVip = true
						};
						if(res.info.data[0].staff&&res.info.data[0].staff[0]&&res.info.data[0].staff[0].info&&res.info.data[0].staff[0].info.phone!=''){
							self.kefuPhone = res.info.data[0].staff[0].info.phone
						}else{
							self.kefuPhone = uni.getStorageSync('user_info').thirdApp.phone
						}
					}
				}
				self.$apis.userGet(postData, callback);
			},
			
			phoneCall() {
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.kefuPhone
				})
			},
			
			showToast1() {
				const self = this;
				self.is_show1 = false
			},
			
			getTipsData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '普通会员提示',
					menu_id: 10
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.tipsData = res.info.data[0]
					}
					console.log('menu', self.tipsData)
					self.$Utils.finishFunc('getTipsData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					menu_id: 7,
					thirdapp_id: 2
				};
				postData.order = {
					publish_time:'desc'
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						/* self.mainData = res.info.data;
						for(var i=0;i<res.info.data.length;i++){
							self.mainData[i].create_time = self.mainData[i].create_time.substring(0,10);
							self.timeData.push(self.mainData[i].create_time)
						}
						for(var j=0;j<self.timeData.length;j++){
							if(self.timeData[j]==self.timeData[j+1]){
								self.timeData.splice(j,1)
							}
						} */
						for (var i = 0; i < res.info.data.length; i++) {
							/* data[i].publish_time = self.$Utils.timeto(data[i].publish_time,'ymd-hms').substring(0,16); */
							res.info.data[i].publish_time = self.$Utils.timeto(res.info.data[i].publish_time,'ymd-hms').substr(0,10)
							if(self.mainData.length>0){
								var hasone = false;
								for(var j =0;j<self.mainData.length;j++){
									if(res.info.data[i].publish_time==self.mainData[j].menu){
										self.mainData[j].data.push(res.info.data[i]);
										hasone = true;
									};
								};
								if(!hasone){
									self.mainData.push({
										menu: res.info.data[i].publish_time,
										data:[res.info.data[i]]
									});
								};
							}else{
								self.mainData.push({
									menu: res.info.data[i].publish_time,
									data:[res.info.data[i]]
								})
							};
						};
					}
					console.log('mainData',self.mainData)
					console.log('timeData',self.timeData)
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
		}
	}
</script>

<style scoped>
.acvCon .tit{width: 530rpx;}
.acvImg{width: 100%;height: 300rpx;margin-top: 20rpx;}
.date::before{content: ''; width: 14rpx;height: 14rpx;position: absolute;top: 40%;right: -8rpx; background-color: #E39423;border-radius: 50%;}
.nav .on {
		color: #222;
	}

	.hot-mask {
		width: 100%;
		height: 100%;
		background-color: #F5F5F5;
		opacity: 0.6;
		position: absolute;
		z-index: 100;
	}

	.tc {
		width: 620rpx;
		margin: auto;
		left: 0;
		right: 0;
		top: 30%;
	}

	.tc .txt {
		padding: 60rpx;
	}

	.tc .btn1 {
		line-height: 100rpx;
	}

	.mhBox {
		filter: blur(1px);
	}

	.mh {
		filter: blur(6px);
	}
</style>
