<template>
	<view class="h-100">
		
		<view class="h-100 p-fXY">
			<view class="left line-h h-100 font-26 color6 p-a bg-f5">
				<view :data-id="'item'+index" v-for="(item,index) in labelData" @tap="bindToView" :class="toView=='item'+index?'on':''" class="py-3 classLi">{{item.title}}</view>
			</view>
			
			<scroll-view class="radius10 bg-white mr-3 mt-3 font-26 right" :scroll-into-view="toView" scroll-y="true" scroll-with-animation="true" @scroll="view" >
				<view class="px-3" :id="'item'+index" v-for="(item,index) in labelData" :class="'item'+index">
					<view class="font-30 font-w pt-5 pb-3">{{item.title}}</view>
					<view class="flex flex-wrap" v-for="">
						<view class="flex4 pb-3 tjItem"
						@click="Router.navigateTo({route:{path:'/pages/shop-classifyGood/shop-classifyGood?id='+c_item.id}})"
						v-for="(c_item,c_index) in item.children" :key="c_index">
							<image :src="c_item.mainImg[0].url" class="classImg mb-2"></image>
							<view>{{c_item.title}}</view>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
		
		
		
		
		
		<!-- <view style="height: 140rpx;"></view> -->
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-index/shop-index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/shop-classify/shop-classify'}})">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
				<view>分类</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-car/shop-car'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
				<view>购物车</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/shop-user/shop-user'}})">
				<image src="../../static/images/nabar4.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				leftCurr:0,
				title:'item0',
				labelData:[],
				list:['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z'],
				toView:'item0',
			}
		},
		onLoad(option){
			const self = this;
			if(option.id){
				self.leftCurr = option.id
			}
			self.$Utils.loadAll(['getLabelData'], self);
		},
		methods: {
			
			bindToView(e){
				var id = e.currentTarget.dataset.id;
				this.toView = id;
			},
			
			view(e){
				const query = uni.createSelectorQuery().in(this);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.order = {
					listorder:'asc'
				}
				postData.searchItem = {
					type: 3
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.labelData =  res.info.data;
					}
					console.log('label',self.labelData);
					self.$Utils.finishFunc('getLabelData');
				}
				self.$apis.labelGet(postData, callback);
			},
			
		}
	}
</script>

<style>
html{height: 100%!important;}
page{height: 100%;background-color: #f5f5f5;}

.left{width: 180rpx;padding-bottom: 100rpx;box-sizing: border-box;overflow-y: auto;}
.left .classLi{text-align: center;margin-bottom: 40rpx;}
.left .on{color: #E39423;position: relative;}
.left .on::before{content: ''; height: 60rpx;width: 14rpx;border-radius: 7rpx;background-color: #E39423;position: absolute;left: -7rpx;top: 14rpx;}
.right{overflow-y: auto;box-sizing: border-box;margin-bottom: 140rpx;margin-left: 180rpx;height: 89%;}
uni-scroll-view{width: auto!important;}

.tjItem{width: 33.33%;}

.classImg{width: 120rpx;height: 120rpx;}
.classItem{width: 33.33%;}
</style>
