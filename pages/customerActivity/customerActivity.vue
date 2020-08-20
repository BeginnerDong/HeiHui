<template>
	<view>
		
		<view class="flex3 pt-1" v-for="(item,index) in timeData" :key="index">
			<view class="pt-3 p-r font-26 pl-3 flex-1 date">
				<text class="font-42">{{item.split('-')[2]}}</text>{{item.split('-')[1]}}月
			</view>
			<view class="px-3 mt-3 bL-f5">
				<block v-for="(c_item,c_index) in mainData" :key="c_index">
					<view class="acvCon pb-3" v-show="c_item.create_time.substring(0,10) == item"
					@click="Router.navigateTo({route:{path:'/pages/detail/detail?menu_id=7&id='+c_item.id}})">
						<view class="font-32 avoidOverflow2 tit">{{c_item.title}}</view>
						<image :src="c_item.mainImg[0].url" class="acvImg"></image>
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
				timeData:[],
				mainData:[]
			}
		},
		onLoad(){
			const self = this;
			uni.setStorageSync('path','/pages/customerActivity/customerActivity')
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					menu_id: 7,
					thirdapp_id: 2
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData = res.info.data;
						for(var i=0;i<res.info.data.length;i++){
							self.mainData[i].create_time = self.mainData[i].create_time.substring(0,10);
							self.timeData.push(self.mainData[i].create_time)
						}
						for(var j=0;j<self.timeData.length;j++){
							if(self.timeData[j]==self.timeData[j+1]){
								self.timeData.splice(j,1)
							}
						}
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
</style>
