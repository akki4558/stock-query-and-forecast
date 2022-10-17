<template>
	<view>
	
	
	<view class="place"></view>
	<view class="exp">
		<text>股票名称:{{name}}</text>
	</view>
	<view class="place"></view>
	<view class="exp">
		 <text>实时股价:</text><text>{{price+"    "}}</text>
	</view>
	<view class="place"></view>
	<view class="exp">
		<text>涨跌百分比:</text><text>{{increPer+"%"}}</text>
	</view>
	<view class="place"></view>
	<view class="exp">
		<text>预测涨幅:</text><text>{{rand[pred]+"%"}}</text>
	</view>
	<view class="place"></view>
	<view class="place2">
		<image class="place2" src="../../static/query/stockQuery.jpg" mode=scaleToFill></image>
	</view>
	<view class="place"></view>
	<view>
		<input class="stockIn" @blur="type" :value="hisCode" placeholder-style="color:#d5d3d4" placeholder="请输入待查询股票代码或名称"/>
		<button v-on:click="search" type="primary" hover-class="button-hover">查询</button>
	</view>
	
	<view class="place8">
	</view>
	
	<view class="row">
	
	<picker mode="selector" :range="history" range-key="name" @change="insert">
			<view class="place9">
			<button class="place6 textHis" size="mini" plain="true" type="warn">历史记录</button>
			</view>
		</picker>
	
	<view class="place7">
		<button @click="askClean" plain="true" type="warn">🗑</button>
	</view>
	</view>	
	
	</view>
	
</template>

<script>
	let stockCode;
	let price;
	let name;
	let hisIndex;
	let hisCode;
	export default{
		data(){
			return {
				price:uni.getStorageSync('stockPrice')+"(未更新)",
				name:uni.getStorageSync('stockName'),
				increPer:0.00,
				hisCode:'',
				rand:[0.32,-1.62,1.29,0.44,-0.76],
				pred:0
				}
			
		}	,
			
		onLoad() {
	     
	     	uni.showLoading({
	     	    title: '加载中'
	     	})
	     	
	     
		 if(''==uni.getStorageSync('hisIndex')){
			this.history=[{'name':'','code':''},{'name':'','code':''},{'name':'','code':''},{'name':'','code':''},{'name':'','code':''}]
			this.hisIndex=0
			uni.setStorageSync('hisIndex',0)
			uni.setStorageSync('history',this.history)
		 }
		 
		this.hisIndex=uni.getStorageSync('hisIndex')
		 this.history=uni.getStorageSync('history')
		 setTimeout(()=>{uni.hideLoading()},700)
		},
		
		methods:{
			type(event){
				stockCode=event.detail.value
				
				
			},
			push(his){
				if(this.hisIndex<5){
				this.history[this.hisIndex].code=his.code
				this.history[this.hisIndex].name=his.name
				this.hisIndex++
				uni.setStorageSync('hisIndex',this.hisIndex)
				uni.setStorageSync('history',this.history)
				
				}
				
			},
			askClean(){
				uni.showModal({
				    title: '提示',
				    content: '确定要清空查询历史吗？',
				    success(res) {
				        if (res.confirm) {
				            this.history=[{'name':'','code':''},{'name':'','code':''},{'name':'','code':''},{'name':'','code':''},{'name':'','code':''}]
				            this.hisIndex=0
				            uni.setStorageSync('hisIndex',0)
							uni.setStorageSync('history',this.history)
				        } 
				    }
				});
			},
			
			
				
				
			
			insert(e){
				this.hisCode=this.history[e.detail.value].code
			},
			search(){
				uni.showLoading({
				    title: '查询中'
				});
				this.pred=(this.pred+1)%5
				uni.request({
					url:'http://web.juhe.cn:8080/finance/stock/hs',
					data: {
					        gid: stockCode,
					        key: '89826c4dd4e319a06814ddb4c1ba2599'
					    },
					success(res) {
						//sh601009
						
						if(res.data.reason=="SUCCESSED!"){
						
						uni.setStorageSync('stockPrice',res.data.result["0"].data.nowPri)
						uni.setStorageSync('stockName',res.data.result["0"].data.name)
						uni.setStorageSync('increPer',res.data.result["0"].data.increPer)
					
						}
						else{
							
						}
					},
				}),
				setTimeout(()=>{
					uni.hideLoading()
					this.name=uni.getStorageSync('stockName')
					this.price=uni.getStorageSync('stockPrice')
					this.increPer=uni.getStorageSync('increPer')
					
					this.push({
						'name':this.name,
						'code':stockCode
						})
						stockCode=''
					},1600)
			
			},
			
		}
	}
</script>

<style>
	@import url("../../static/My.css");
</style>