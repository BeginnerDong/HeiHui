<template>
	<view class="pb-3">
		
		<view class="flex1 bB-e1 bg-white p-3">
			<view class="p-r ssBox">
				<input type="text" v-model="keywords" auto-focus="true"  placeholder="搜索你想找的" class="ss" />
				<image src="../../static/images/search-icon.png" class="ss-icon p-a"></image>
			</view>
			<view class="colorM" @click="search()">搜索</view>
		</view>
		
		<block v-for="(item,index) in mainData" :key="index">
			<view class="article mt-3 mx-3 shadowM radius10 px-2 py-3 flex1" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/detail/detail?type='+self.searchItem.type+'&id='+$event.currentTarget.dataset.id}})">
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
				mainData:[],
				keywords:'',
				searchItem:{
					
				}
			}
		},
		
		onLoad(options){
			const self = this;
			var options = self.$Utils.getHashParameters();
			console.log(options);
			self.searchItem.type = options[0].type;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			search(){
				const self = this;
				if(self.keywords!=''){
					self.searchItem.title = ['LIKE', ['%' + self.keywords + '%']]
					self.getMainData(true)
				}else{
					self.$Utils.showToast('请输入关键词搜索','none');
				}
			},
			
			getMainData(isNew) {
				const self = this;
				const postData = {};
				if(isNew){
					self.mainData = []
				}
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data;
						for(var i=0;i<res.info.data.length;i++){
							self.mainData[i].create_time = self.mainData[i].create_time.substring(0,10);
						}
					}
					console.log('mainData',self.mainData)
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
.ss-icon{top: 25%;left: 35%;}
.ss{flex: 1;width: 600rpx;margin: 0;}
.ssBox{position: sticky;top: 0;}
</style>
