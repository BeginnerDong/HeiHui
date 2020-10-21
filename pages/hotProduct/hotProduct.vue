<template>
	<view>
	
		<view class="font-26 color6 flex1 p-3 nav" :class="navCurr==0?'bg-white':''">
			<view class="flex4" @click="changeNav(0)" :class="navCurr==0?'on':''">
				<image src="../../static/images/product-icon.png" class="hot-icon" v-if="navCurr==0"></image>
				<image src="../../static/images/product-icon12.png" class="hot-icon" v-else></image>
				<view>全部</view>
			</view>
			<view class="flex4" @click="changeNav(1)" :class="navCurr==1?'on':''">
				<image src="../../static/images/product-icon13.png" class="hot-icon" v-if="navCurr==1"></image>
				<image src="../../static/images/product-icon1.png" class="hot-icon" v-else></image>
				<view>地产投资</view>
			</view>
			<view class="flex4" @click="changeNav(2)" :class="navCurr==2?'on':''">
				<image src="../../static/images/product-icon15.png" class="hot-icon" v-if="navCurr==2"></image>
				<image src="../../static/images/product-icon2.png" class="hot-icon" v-else></image>
				<view>工商企业</view>
			</view>
			<view class="flex4" @click="changeNav(3)" :class="navCurr==3?'on':''">
				<image src="../../static/images/product-icon16.png" class="hot-icon" v-if="navCurr==3"></image>
				<image src="../../static/images/product-icon3.png" class="hot-icon" v-else></image>
				<view>现金管理</view>
			</view>
			<view class="flex4" @click="changeNav(4)" :class="navCurr==4?'on':''">
				<image src="../../static/images/product-icon14.png" class="hot-icon" v-if="navCurr==4"></image>
				<image src="../../static/images/product-icon4.png" class="hot-icon" v-else></image>
				<view>其他</view>
			</view>
		</view>



		<view>
			<!-- 其他 -->
			<view class="hot m-3 shadowM radius10 overflow-h font-24 p-r bg-white" 
			@click="toDetail(index)" v-for="(item,index) in mainData"
			:key="index"
			:class="isVip||item.top==1||userInfoData.deadline>Date.parse(new Date())/1000?'':'mhBox'">
				<!-- <view class="hot-mask" v-show="!isVip"></view> -->
				<image src="../../static/images/product-icon11.png" class="hotBg"></image>
				<view class="colorf d-flex px-2 p-r py-2 ">
					<view class="font-22 sqSgin" v-if="item.sellout==1">售罄</view>
					<view class="font-22 hotSgin" style="margin-left: 10rpx;" v-if="item.hot==1">热门</view>
					<view class="font-22 zdSgin" style="margin-left: 10rpx;" v-if="item.top==1">置顶</view>
					<view class="font-22 yxSgin" style="margin-left: 10rpx;width: 90rpx;" v-if="item.run_status==0">运行中</view>
					<view class="font-22 zdSgin" style="margin-left: 10rpx;width: 90rpx;" v-if="item.run_status==1">募集中</view>
					<view class="font-22 sqSgin" style="margin-left: 10rpx;width: 90rpx;" v-if="item.run_status==2">已退出</view>
					
					<view class="qySgin">{{item.label[item.menu_id].title}}</view>
				</view>
				<view class="colorf d-flex px-2 p-r pb-2  borderDB">
					<view class="font-32 color2 pl-1" :class="isVip||item.top==1||userInfoData.deadline>Date.parse(new Date())/1000?'':'mh'">{{item.title}}</view>
				</view>
				
				<view class="flex3 pt-5 px-2 pb-4">
					<view class="flex4">
						<view class="flex1 pb-1">
							<image src="../../static/images/product-icon5.png" class="yq-icon"></image>
							<view>业绩比较基准</view>
						</view>
						<view class="colorR font-50 font-w" :class="isVip||item.top==1||userInfoData.deadline>Date.parse(new Date())/1000?'':'mh'">{{item.small_title}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon6.png" class="yq-icon1"></image>
							<view>期限(月)</view>
						</view>
						<view class="font-40" :class="isVip||item.top==1||userInfoData.deadline>Date.parse(new Date())/1000?'':'mh'">{{item.keywords}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon7.png" class="yq-icon1"></image>
							<view>起投金额(万)</view>
						</view>
						<view class="font-40" :class="isVip||item.top==1||userInfoData.deadline>Date.parse(new Date())/1000?'':'mh'">{{item.description}}</view>
					</view>
				</view>
			</view>

		</view>



		<!-- 重要提示 -->
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-r overflow-h tips">
				<image src="../../static/images/product-icon8.png" class="x-icon" style="margin: 20px;" @click="close()"></image>
				<view class="flex0 font-30 font-w pb-3 pt-3 a-center" style="border-bottom: 1px solid #E5E5E5;">
					<image src="../../static/images/product-icon9.png" class="tips-icon"></image>
					<view>{{noteData.title}}</view>
				</view>
				<view class="font-26 pt-2 mx-3" style="overflow-y: auto;height: 500px;padding-bottom: 150px;">
					<view class="content ql-editor" style="padding:0;" v-html="noteData.content">
						
					</view>
				</view>
				<view class="p-a bottom-0 left-0 right-0">
					<!-- <view class="colorf font-24 pb-1">
						<image src="../../static/images/product-icon10.png" class="tips-icon1"></image>
						<view class="time">{{text}}</view>
					</view> -->
					<view class="tipBtn" @click="close()" style="background:#E39423">确定</view>
				</view>
			</view>
		</view>
		
		<view class="bg-mask" v-show="is_show1">
			<view class="bg-white p-a radius10 text-center tc">
				<view class="txt">
					<view class="content ql-editor text-center" style="padding:0;" v-html="tipsData.content">
						
					</view>
				</view>
				<view class="" style="margin-bottom: 30rpx;color: #888;">联系客服：{{kefuPhone}}</view>
				<view class="flex1 bT-f5 btn">
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
				navCurr: 0,
				is_show: true,
				is_show1:false,
				mainData: [],
				topData: [],
				hotData: [],
				type: {
					tableName: 'Label',
					searchItem: {
						relation_two: 'id'
					},
					fixSearchItem: {
						status: 1,
						relation_one_table: 'Article',
						relation_two_table: 'Label',
					},
					key: 'relation_one',
					middleKey: 'id',
					condition: '=',
				},
				getBefore: {},
				isVip:false,
				noteData:{},
				text:'倒计时10秒',
				currentTime:10,
				hasView:false,
				dataA:[],
				searchItem:{
					thirdapp_id:2,
					type:2
				},
				kefuData:{},
				tipsData:{},
				kefuPhone:'',
				userInfoData:{}
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			uni.setStorageSync('path','/pages/hotProduct/hotProduct')
			self.$Utils.loadAll(['getMainData','getNoteData','getKefuData','getTipsData'], self);
		},
		
		
		
		onShow() {
			const self = this;
			self.getUserInfoData()
		},
		
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getKefuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id: 3,
					thirdapp_id: 2
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.kefuData = res.info.data[0];
					}
					self.$Utils.finishFunc('getKefuData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			phoneCall(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber:self.kefuPhone
				})
			},
			
			showToast1(){
				const self = this;
				self.is_show1 = false
			},
			
			close(){
				const self = this;
				self.is_show = false
				//clearInterval(self.interval)
			},
			
			toDetail(index){
				const self = this;
				self.is_show = false;
				self.willId = self.mainData[index].id;
				if(self.mainData[index].top==1){
					self.$Router.navigateTo({
						route: {
							path: '/pages/detail/detail?type=2&id=' + self.willId
						}
					})
					return
				};
				if(self.isVip){
					self.$Router.navigateTo({
						route: {
							path: '/pages/detail/detail?type=2&id=' + self.willId
						}
					})
				}else{
					if(self.userInfoData.deadline>0&&self.userInfoData.deadline>Date.parse(new Date())/1000){
						self.$Router.navigateTo({
							route: {
								path: '/pages/detail/detail?type=2&id=' + self.willId
							}
						})
					}else if(self.userInfoData.deadline>0&&self.userInfoData.deadline<Date.parse(new Date())/1000){
						self.is_show1 = true;
						return
					}else if(self.userInfoData.deadline==0){
						self.is_show1 = true;
						return
						/* const postData = {};
						postData.tokenFuncName = 'getUserToken';
						postData.data = {
							deadline:Date.parse(new Date())/1000 + uni.getStorageSync('user_info').thirdApp.temporary*60 
						};
						var callback = function(res) {
							if (res.solely_code == 100000) {
								self.$Router.navigateTo({
									route: {
										path: '/pages/detail/detail?type=2&id=' + self.willId
									}
								})
							}
						};
						self.$apis.userInfoUpdate(postData, callback); */
					};
				}	
			},
			
			getTipsData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'普通会员提示',
					menu_id: 10
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.tipsData = res.info.data[0]
					}
					console.log('menu',self.tipsData)
					self.$Utils.finishFunc('getTipsData');
				}
				self.$apis.articleGet(postData, callback);
			},
			
			getNoteData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'重要须知'
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.noteData = res.info.data[0]
					}
					self.$Utils.finishFunc('getNoteData');
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
						if (self.userInfoData.behavior == 1 || self.userInfoData.user_type == 1) {
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

			changeNav(i) {
				const self = this;
				if(self.navCurr != i){
					self.navCurr = i;
					if(self.navCurr==0){
						delete self.searchItem.menu_id 
					}else if(self.navCurr==1){
						self.searchItem.menu_id = 11
					}else if(self.navCurr==2){
						self.searchItem.menu_id = 12
					}else if(self.navCurr==3){
						self.searchItem.menu_id = 13
					}else if(self.navCurr==4){
						self.searchItem.menu_id = 14
					};
					self.getMainData(true)
				}
				
			},

			getMainData(isNew) {
				const self = this;
				const postData = {};
				if (isNew) {
					
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage:1,
						pagesize:10,
						is_page:true,
					}
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.order = {
					/* top:'desc',
					hot:'desc',
					sellout:'asc', */
					listorder:'asc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						
						self.mainData.push.apply(self.mainData,res.info.data)
						/* console.log('self.dataA',self.dataA)
						for (var i = 0; i < self.dataA.length; i++) {
							if (self.dataA[i].hot == 1) {
								var hot = [];
								hot.push(self.dataA[i]);
								self.hotData = hot
							} else if (self.dataA[i].top == 1) {
								var top = [];
								top.push(self.dataA[i]);
								self.topData = top
							} else if (self.dataA[i].top == 0 && self.dataA[i].hot == 0) {
								self.mainData.push(self.dataA[i])
							}
						} */
						// console.log('hot',self.hotData);
						// console.log('top',self.topData);
						// console.log('main',self.mainData);
						// console.log('label',self.hotData.Label);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}

		},

	}
</script>

<style>
	page {
		background-color: #f5f5f5;
	}

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
	.tc{width: 620rpx;margin: auto;left: 0;right: 0;top: 30%;}
	.tc .txt{padding: 60rpx;}
	.tc .btn{line-height: 100rpx;}
	
	.mhBox{filter: blur(1px);}
	.mh{filter: blur(6px);}
</style>
