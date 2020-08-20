<template>
	<view>
		
		<view class="p-r"  @click="Router.navigateTo({route:{path:'/pages/serach/serach?type=5'}})">
			<input type="text" value="" placeholder="搜索你想找的" class="ss"/>
			<image src="../../static/images/search-icon.png" class="ss-icon p-a"></image>
		</view>
		
		<view class="list flexX">
			<view class="li" :class="liCurr==index?'on':''" @click="changeLi(index,item.id)"
			v-for="(item,index) in labelData" :key="index"
			:style="{width: 100/labelData.length+'%'}">{{item.title}}</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="flexX mx-2 px-1">
			<view class="mt-3 mb-2 mr-2 p-r radius10 shadow overflow-h flex-shrink" v-show="index<5"
			v-for="(item,index) in topData" :key="index"
			@click="Router.navigateTo({route:{path:'/pages/detail/detail?type=5&id='+item.id}})">
				<image :src="item&&item.mainImg&&item.mainImg[0]&&item.mainImg[0].url" class="trustImg"></image>
				<view class="font-32 p-3 p-aX bottom-0 bg-white text-center">{{item.title}}</view>
			</view>
		</view>
		
		<view class="article mx-3 radius10 py-3 flex1 bB-f5"
		v-for="(item,index) in mainData" :key="index"
		@click="Router.navigateTo({route:{path:'/pages/detail/detail?type=5&id='+item.id}})">
			<view class="flex5 flex-1">
				<view class="tit font-32 avoidOverflow2 flex-1">恒辉·{{item.label[item.menu_id].title.substring(0,2)}} | {{item.title}}</view>
				<view class="font-24 color9 pt-2">
					<text class="artSgin">#{{item.label[item.menu_id].title}}</text>
				</view>
			</view>
			<image src="../../static/images/about-img2.png" class="artImg"></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0,
				topData:[],
				mainData:[],
				searchItem:{
					type: 5,
					thirdapp_id: 2
				},
				labelData:[]
			}
		},
		
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/encyclopedias/encyclopedias')
			self.$Utils.loadAll(['getLabelData'], self);
		},
		
		methods: {
			
			changeLi(index,id){
				const self = this;
				self.liCurr = index;
				// self.searchItem.menu_id = id;
				self.getMainData(true,id);
			},
			
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parentid : 8
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
					self.topData = [];
					self.mainData = [];
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						var data = res.info.data;
						for(var i=0;i<data.length;i++){
							data[i].create_time = data[i].create_time.substring(0,10);
							if(data[i].top == 1){
								self.topData.push(data[i])
							}else if(data[i].top == 0 &&  data[i].menu_id == id){
								self.mainData.push(data[i])
							}
						}
						console.log('top',self.topData);
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
.li{padding: 0 30rpx;flex-shrink: 0;}
.trustImg{width: 690rpx;height: 260rpx;}
.artImg{width: 175rpx;height: 140rpx;}
.article .tit{width: 455rpx;}
</style>
