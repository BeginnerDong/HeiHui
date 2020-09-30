<template>
	<view>
		
		<view class="p-r colorf">
			<image src="../../static/images/personal center-img.png" class="perBg"></image>
			
			<view class="flex1 h-100 p-a top-0 px-3">
				<image style="border-radius: 50%;overflow: hidden;" :src="userData.headImgUrl?userData.headImgUrl:'../../static/images/head.png'" class="userImg"></image>
				<view class="flex-1 font-32 pl-2">{{userData?userData.nickname:''}}</view>
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
			<view class="article mt-3 mx-3 shadowM radius10 px-2 py-3 flex1" 
			v-for="(item,index) in mainData" :key="index"
			@click="Router.navigateTo({route:{path:'/pages/detail/detail?menu_id=2&id='+item.article[0].id}})">
				<view class="flex5" style="height: 160rpx;">
					<view class="tit font-30 avoidOverflow3">{{item.article&&item.article[0]?item.article[0].title:''}}</view>
					<view class="font-24 color9 pt-2">
						<!-- <text class="artSgin">#</text> -->
						{{item.article&&item.article[0]?item.article[0].publish_time:''}}
					</view>
				</view>
				<image :src="item.article&&item.article[0]&&item.article[0].mainImg&&item.article[0].mainImg[0]?item.article[0].mainImg[0].url:'../../static/images/null.png'" class="artImg"></image>
			</view>
		</view>
		
		<view v-show="liCurr==1">
			<view class="hot m-3 shadowM radius10 overflow-h font-24 p-r bg-white" 
			v-for="(item,index) in mainData" :key="index"
			@click="Router.navigateTo({route: { path: '/pages/detail/detail?type=2&id=' + item.article[0].id } })">
				<image src="../../static/images/product-icon11.png" class="hotBg"></image>
				<view class="colorf d-flex px-2 p-r py-4 borderDB">
					<!-- <view class="font-22 hotSgin">热门</view> -->
					<view class="font-32 color2 pl-1 tit">{{item.article&&item.article[0]?item.article[0].title:''}}</view>
					<view class="qySgin">
						{{item.article&&item.article[0]&&item.article[0].label&&item.article[0].label[item.article[0].menu_id]?item.article[0].label[item.article[0].menu_id].title:''}}
					</view>
				</view>
				<view class="flex3 pt-5 px-2 pb-4">
					<view class="flex4">
						<view class="flex1 pb-1">
							<image src="../../static/images/product-icon5.png" class="yq-icon"></image>
							<view>预期收益最高可达</view>
						</view>
						<view class="colorR font-50 font-w">{{item.article&&item.article[0]?item.article[0].small_title:''}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon6.png" class="yq-icon1"></image>
							<view>项目期限(月)</view>
						</view>
						<view class="font-40">{{item.article&&item.article[0]?item.article[0].keywords:''}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon7.png" class="yq-icon1"></image>
							<view>起投金额(万)</view>
						</view>
						<view class="font-40">{{item.article&&item.article[0]?item.article[0].description:''}}</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view @click="loginOff" class="addBto text-center flex0 colorM line-h bT-f5" style="position: fixed;bottom: 0;width: 100%;height: 100rpx;background-color: #E39423;color: #fff;">
			<view>退出登录</view>
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
				is_show:false,
				userData:{},
				searchItem:{
					status:1,
					behavior:1,
					
				},
				mainData:[]
			}
		},
		
		
		onLoad() {
			const self = this;
			uni.setStorageSync('path','/pages/personal/personal')
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserData','getMainData'], self);
		},
		
		
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			loginOff(){
				const self = this;
				uni.removeStorageSync('user_info');
				uni.removeStorageSync('user_token');
				self.Router.redirectTo({route:{path:'/pages/login-register/login-register'}})
			},
			
			changeLi(i){
				const self = this;
				if(self.liCurr!=i){
					self.liCurr = i;
					if(self.liCurr==0){
						self.searchItem.behavior=1
					}else if(self.liCurr==1){
						self.searchItem.behavior=2
					};
					self.getMainData(true)
				}
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			},
			 
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				var callback = function(res){
					if(res.info.data.length > 0){
						self.userData = res.info.data[0];
					}
					self.$Utils.finishFunc('getUserData');
				}
				self.$apis.userGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage:1,
						pagesize:10,
						is_page:true,
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.getAfter = {
					article:{
						tableName:'Article',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData.push.apply(self.mainData,res.info.data)
						for (var i = 0; i < self.mainData.length; i++) {
							if(self.mainData[i].article&&self.mainData[i].article[0]){
								self.mainData[i].article[0].publish_time = self.$Utils.timeto(self.mainData[i].article[0].publish_time,'ymd-hms').substr(0,10)
							}
						}
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.logGet(postData, callback);
			},
			
		}
	}
</script>

<style>
.perBg{width: 750rpx;height: 200rpx;}

.li{width: 50%;}

</style>
