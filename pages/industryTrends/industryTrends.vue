<template>
	<view>
		
		<view class="p-r">
			<input type="text" value="" placeholder="搜索你想找的" class="ss"/>
			<image src="../../static/images/search-icon.png" class="ss-icon p-a"></image>
		</view>
		
		<view class="list flexX">
			<view class="li" :class="liCurr==index?'on':''" @click="changeLi(index,item.id)"
			v-for="(item,index) in labelData" :key="index"
			:style="{width: 100/labelData.length+'%'}">{{item.title}}</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<block v-for="(item,index) in mainData" :key="index">
			<view class="article mt-3 mx-3 shadowM radius10 px-2 py-3 flex1"
			@click="Router.navigateTo({route:{path:'/pages/detail/detail?type=4&id='+item.id}})">
				<view style="height: 160rpx;" class="flex5">
					<view class="tit font-30 flex-1 avoidOverflow3">{{item.title}}</view>
					<view class="font-24 color9 pt-2">
						<text class="artSgin">#{{item.label[item.menu_id].title}}</text> {{item.create_time}}
					</view>
				</view>
				<image src="../../static/images/about-img2.png" class="artImg"></image>
			</view>
		</block>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0,
				mainData:[],
				searchItem:{
					type: 4,
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
			
			changeLi(index,id){
				const self = this;
				self.liCurr = index;
				self.searchItem.menu_id = id;
				self.getMainData(true,id);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					parentid : 6
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData =  res.info.data;
					}
					console.log('label',self.labelData);
					self.searchItem.menu_id = self.labelData[0].id;
					self.getMainData(true,self.labelData[0].id);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew,id) {
				const self = this;
				if (isNew) {
					self.mainData = [];
				};
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data;
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
.trustImg{width: 690rpx;height: 340rpx;}
</style>
