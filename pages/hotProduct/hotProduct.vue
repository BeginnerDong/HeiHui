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
				<view>房地产投资</view>
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
			<!-- 置顶 -->
			<view class="hot m-3 shadowM radius10 overflow-h font-24 p-r bg-white" @click="isShow(topData.id)">
				<image src="../../static/images/product-icon11.png" class="hotBg"></image>
				<view class="colorf d-flex px-2 p-r py-4 borderDB">
					<view class="font-22 zdSgin">置顶</view>
					<view class="font-32 color2 pl-1 tit">{{topData.title}}</view>
					<view class="qySgin">{{topData.label[topData.menu_id].title}}</view>
				</view>
				<view class="flex3 pt-5 px-2 pb-4">
					<view class="flex4">
						<view class="flex1 pb-1">
							<image src="../../static/images/product-icon5.png" class="yq-icon"></image>
							<view>预期收益最高可达</view>
						</view>
						<view class="colorR font-50 font-w">{{topData.small_title}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon6.png" class="yq-icon1"></image>
							<view>项目期限(月)</view>
						</view>
						<view class="font-40">{{topData.keywords}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon7.png" class="yq-icon1"></image>
							<view>起投金额(万)</view>
						</view>
						<view class="font-40">{{topData.description}}</view>
					</view>
				</view>
			</view>
			<!-- 热门 -->
			<view class="hot m-3 shadowM radius10 overflow-h font-24 p-r bg-white" @click="isShow(hotData.id)">
				<image src="../../static/images/product-icon11.png" class="hotBg"></image>
				<view class="colorf d-flex px-2 p-r py-4 borderDB">
					<view class="font-22 hotSgin">热门</view>
					<view class="font-32 color2 pl-1 tit">{{hotData.title}}</view>
					<view class="qySgin">{{hotData.label[hotData.menu_id].title}}</view>
				</view>
				<view class="flex3 pt-5 px-2 pb-4">
					<view class="flex4">
						<view class="flex1 pb-1">
							<image src="../../static/images/product-icon5.png" class="yq-icon"></image>
							<view>预期收益最高可达</view>
						</view>
						<view class="colorR font-50 font-w">{{hotData.small_title}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon6.png" class="yq-icon1"></image>
							<view>项目期限(月)</view>
						</view>
						<view class="font-40">{{hotData.keywords}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon7.png" class="yq-icon1"></image>
							<view>起投金额(万)</view>
						</view>
						<view class="font-40">{{hotData.description}}</view>
					</view>
				</view>
			</view>
			<!-- 其他 -->
			<view class="hot m-3 shadowM radius10 overflow-h font-24 p-r bg-white" @click="isShow(item.id)"
			v-for="(item,index) in mainData" :key="index">
				<view class="hot-mask" v-show="navCurr!=0"></view>
				<image src="../../static/images/product-icon11.png" class="hotBg"></image>
				<view class="colorf d-flex px-2 p-r py-4 borderDB">
					<view class="font-32 color2 pl-1 tit">{{item.title}}</view>
					<view class="qySgin">{{item.label[item.menu_id].title}}</view>
				</view>
				<view class="flex3 pt-5 px-2 pb-4">
					<view class="flex4">
						<view class="flex1 pb-1">
							<image src="../../static/images/product-icon5.png" class="yq-icon"></image>
							<view>预期收益最高可达</view>
						</view>
						<view class="colorR font-50 font-w">{{item.small_title}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon6.png" class="yq-icon1"></image>
							<view>项目期限(月)</view>
						</view>
						<view class="font-40">{{item.keywords}}</view>
					</view>
					<view class="flex4">
						<view class="flex1 pb-2">
							<image src="../../static/images/product-icon7.png" class="yq-icon1"></image>
							<view>起投金额(万)</view>
						</view>
						<view class="font-40">{{item.description}}</view>
					</view>
				</view>
			</view>
			
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
				navCurr:0,
				is_show:false,
				mainData:[],
				topData:[],
				hotData:[],
				type:{
				  tableName:'Label',
				  searchItem:{
						relation_two:'id'
				  },
				  fixSearchItem:{
				    status:1,
						relation_one_table:'Article',
						relation_two_table:'Label',
				  },
				  key:'relation_one',
				  middleKey:'id',
				  condition:'=',
				},
				getBefore:{}
			}
		},
		onLoad(){
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			changeNav(i){
				const self = this;
				self.navCurr = i;
			},
			
			isShow(id){
				const self = this;
				self.is_show = !self.is_show
				self.$Router.navigateTo({route:{path:'/pages/detail/detail?type=2&id='+id}})
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				// postData.tokenFuncName = 'getProjectToken';
				// postData.getBefore = self.$Utils.cloneForm(self.getBefore);
				postData.searchItem = {
					type: 2,
					thirdapp_id: 2
				};
				var callback = function(res){
					if(res.info.data.length > 0){
						var data = res.info.data;
						for(var i=0;i<data.length;i++){
							if(data[i].hot == 1){
								var hot = [];
								hot.push(data[i]);
								self.hotData = hot[0]
							}else if(data[i].top == 1){
								var top = [];
								top.push(data[i]);
								self.topData = top[0]
							}else if(data[i].top == 0&&data[i].hot == 0){
								self.mainData.push(data[i])
							}
						}
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
page{background-color: #f5f5f5;}
.nav .on{color: #222;}
.hot-mask{width: 100%;height: 100%;background-color:  #F5F5F5;opacity: 0.6;position: absolute;z-index: 100;}
</style>
