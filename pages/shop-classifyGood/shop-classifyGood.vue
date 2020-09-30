<template>
	<view>
		
		<view class="good flex1 px-3 pt-4 pb-3 bB-f5"
		v-for="(item,index) in mainData" :key="index"
		:data-id="item.id"
		@click="Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail?id='+$event.currentTarget.dataset.id}})">
			<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="goodImg radius10"></image>
			<view class="flex-1 pl-2">
				<view class="font-30 flex-1 pb-4 avoidOverflow2">{{item.title}}</view>
				<view class="line-h flex1 pt-4">
					
					<view class="">
						<text class="price1 font-w  font-30">{{item.sku&&item.sku[0]?item.sku[0].o_price:''}}</text>
						<text class="font-w font-24 priceV"  style="color: #000000;margin-left:20rpx;">¥{{item.sku&&item.sku[0]?item.sku[0].price:''}}</text>
					</view>
					<view class="font-20 color9">销量 {{item.sale_count}}</view>
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
				searchItem:{
					thirdapp_id: 2,
					type: 1
				},
				mainData:[],
				type:'',
				isLoadAll:false
			}
		},
		onLoad(options){
			const self = this;
			if(options.type){
				self.searchItem[options.type] = 1;
			}
			if(options.id){
				self.searchItem.category_id = options.id;
			}
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			goDetail(item){
				const self = this;
				uni.setStorageSync('productDetail',item)
				self.Router.navigateTo({route:{path:'/pages/shop-detail/shop-detail?id='+item.id}})
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.paginate);
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'category_id',
						key:'category_id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				var callback = function(res){
					if(res.info.data.length > 0){
						self.mainData.push.apply(self.mainData,res.info.data)
						console.log('main',self.mainData);
					}
					self.$Utils.finishFunc('getMainData');
				}
				self.$apis.productGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
.goodImg{width: 200rpx;height: 200rpx;}
</style>
