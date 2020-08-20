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
					<view class="newCon py-3 bB-e1"
					@click="Router.navigateTo({route:{path:'/pages/detail/detail?menu_id=2&id='+c_item.id}})">
						<view class="font-32 tit avoidOverflow2">{{c_item.title}}</view>
						<image :src="c_item.mainImg[0].url" class="newImg"></image>
					</view>
				</block>
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
				mainData:[],
				timeData:[]
			}
		},
		onLoad(){
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					menu_id: 2,
					thirdapp_id: 2
				};
				var callback = function(res){
					/* if(res.info.data.length > 0){
						self.mainData = res.info.data;
						for(var i=0;i<res.info.data.length;i++){
							self.mainData[i].create_time = self.mainData[i].create_time.substring(0,10);
							self.timeData.push(self.mainData[i].create_time)
						}
						for(var j=0;j<self.timeData.length;j++){
							if(self.timeData[j]==self.timeData[j+1]){
								self.timeData.splice(j,1)
							}
								self.timeData[j] = self.timeData[j].split('-')
						}
					} */
					if (res.info.data.length > 0) {
						for (var i = 0; i < res.info.data.length; i++) {
							res.info.data[i].create_time = res.info.data[i].create_time.substr(0,10)
							if(self.mainData.length>0){
								var hasone = false;
								for(var j =0;j<self.mainData.length;j++){
									if(res.info.data[i].create_time==self.mainData[j].menu){
										self.mainData[j].data.push(res.info.data[i]);
										hasone = true;
									};
								};
								if(!hasone){
									self.mainData.push({
										menu: res.info.data[i].create_time,
										data:[res.info.data[i]]
									});
								};
							}else{
								self.mainData.push({
									menu: res.info.data[i].create_time,
									data:[res.info.data[i]]
								})
							};
						};
						console.log(self.mainData)
						
						//self.mainData.push.apply(self.mainData,res.info.data)
					}
					/* console.log('mainData',self.mainData)
					console.log('timeData',self.timeData) */
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.articleGet(postData, callback);
			}
			
		}
	}
</script>

<style>
.newDate{width: 130rpx;}
.newCon .tit{width: 560rpx;}
.newImg{width: 100%;height: 300rpx;margin-top: 20rpx;}
</style>

							var time = self.mainData[i].create_time.split('-')