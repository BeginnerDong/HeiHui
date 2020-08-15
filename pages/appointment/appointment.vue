<template>
	<view>
		<view>
			<image :src="nowData&&nowData.mainImg&&nowData.mainImg[0]&&nowData.mainImg[0].url" class="appoinImg"></image>
		</view>
		
		<view class="flex py-2 px-3">
			<image src="../../static/images/about-icon.png" class="app-icon"></image>
			<view class="font-32 font-w">视频预告</view>
		</view>
		<view class="flex px-3">
			<video :src="nowData&&nowData.bannerImg&&nowData.bannerImg[0]&&nowData.bannerImg[0].url" class="spImg" controls=""></video>
			<view class="pl-2 flex-1 flex5 spTxt">
				<view class="avoidOverflow2 flex-1">主题：{{nowData.title}}</view>
				<view class="pb-1">时间：2020-07-03 20:00</view>
				<view>专家：{{nowData.small_title}}</view>
			</view>
		</view>
		<view class="font-24 px-3 pt-3">
			<view class="content ql-editor" style="padding:0;" v-html="nowData.content">
				
			</view>
		</view>
		
		<view class="flex py-2 px-3">
			<image src="../../static/images/about-icon.png" class="app-icon"></image>
			<view class="font-32 font-w">往期视频回放</view>
		</view>
		<view class="font-24 line-h flex2 pb-4">
			<view class="flex4" @click="changeLi(0,labelData[0].id)">
				<image src="../../static/images/baike-icon3.png" class="app-icon1" v-show="liCurr!=0"></image>
				<image src="../../static/images/baike-icon4.png" class="app-icon1" v-show="liCurr==0"></image>
				<view>资产配置</view>
			</view>
			<view class="flex4" @click="changeLi(1,labelData[1].id)">
				<image src="../../static/images/baike-icon2.png" class="app-icon1" v-show="liCurr!=1"></image>
				<image src="../../static/images/baike-icon5.png" class="app-icon1" v-show="liCurr==1"></image>
				<view>家族财富管理</view>
			</view>
			<view class="flex4" @click="changeLi(2,labelData[2].id)">
				<image src="../../static/images/baike-icon1.png" class="app-icon1" v-show="liCurr!=2"></image>
				<image src="../../static/images/baike-icon6.png" class="app-icon1" v-show="liCurr==2"></image>
				<view>其他</view>
			</view>
		</view>
		
		<view class="flex1 mx-3 pb-3 p-r"
		v-for="(item,index) in mainData" :key="index"
		@click="goDetail(item)">
			<image :src="item.mainImg[0].url" class="spImg"></image>
			<view class="pl-2 py-1 flex5 wqTxt">
				<view class="font-30 font-w avoidOverflow2">{{item.title}}</view>
				<view class="font-26 pt-3 avoidOverflow2">简介：{{item.description}}</view>
			</view>
			<view class="font-22 p-1 Mgb colorf line-h p-a top-0 left-0">专家 | {{item.small_title}}</view>
		</view>
		
		
		<!-- 弹框 -->
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white mx-3 radius10 text-center font-30 colorf p-r tcBox">
				<image src="../../static/images/x-icon.png" class="x-icon" @click="isShow"></image>
				<view class="Mgb tit">请输入密码</view>
				<view class="py-5">
					<input type="text" value="" placeholder="姓名" />
					<input type="text" value="" placeholder="密码" />
					<view class="btn600" >进入</view>
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
				nowData:{},
				mainData:[],
				searchItem:{
					type: 6,
					thirdapp_id: 2
				},
				labelData:[]
			}
		},
		onLoad(){
			const self = this;
			self.$Utils.loadAll(['getLabelData'], self);
		},
		methods: {
			
			goDetail(item){
				const self = this;
				uni.setStorageSync('appointmentData',item)
				self.Router.navigateTo({route:{path:'/pages/appointmentDetail/appointmentDetail'}})
			},
			
			changeLi(i,id){
				const self = this;
				self.liCurr = i;
				self.getMainData(true,id);
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parentid : 9
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData =  res.info.data;
					}
					console.log('label',self.labelData);
					// self.searchItem.menu_id = self.labelData[0].id;
					self.getMainData(true,self.labelData[0].id);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew,id) {
				const self = this;
				if (isNew) {
					self.nowData = [];
					self.mainData = [];
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						var data = res.info.data;
						console.log('data',data)
						for(var i=0;i<data.length;i++){
							data[i].create_time = data[i].create_time.substring(0,16);
							if(data[i].top == 1){
								self.nowData = data[i]
							}else if(data[i].top == 0 &&  data[i].menu_id == id){
								self.mainData.push(data[i])
							}
						}
						console.log('top',self.nowData);
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
.appoinImg{width: 100%;height: 400rpx;}
.app-icon{width: 30rpx;height: 31rpx;margin-right: 10rpx;}
.app-icon1{width: 110rpx;height: 110rpx;margin-bottom: 30rpx;}
.spImg{width: 260rpx;height: 180rpx;}
.spTxt{height: 180rpx;}
.spTxt view{width: 410rpx;}
.wqTxt view{width: 408rpx;}
.wqTxt{height: 180rpx;}

.tcBox{margin-top: 40%;overflow: hidden;}
.tcBox .tit{line-height: 80rpx;}
.tcBox input{width: 600rpx;height: 80rpx;font-size: 26rpx;margin: 0 auto 30rpx;border: 1px solid #e1e1e1;border-radius: 10rpx;}
.btn600{line-height: 70rpx;}
</style>
