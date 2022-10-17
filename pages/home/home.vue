<template>
	<view>
	<view>
		<image class=midImg src="../../static/stock.png"></image>
		
		<button @click="stoQuery" class="stoBut" type="primary">股价查询</button>
		
	</view>
	<view class="row">
	<view class="col">
		<image class="lowImg" src="../../static/phone.png" mode="center"></image>
		<button @click="phoQuery" size="mini" type="primary">号码归属地</button>
	</view>
	<view class="col">
		<image class="lowImg" src="../../static/news.png" mode="center"></image>
		<button @click="news" type="primary"v size="mini">新闻头条</button>
	</view>
	</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				msg1:'hhh',
				}
			
		}	,	
		onLoad() {
			uni.showLoading({
			    title: '加载中'
			})
			setTimeout(()=>{uni.hideLoading()()},700)
		},
		methods:{
			phoQuery(){
				uni.navigateTo({
					url:'../detail/phone'
				})
				//console.log('按钮已按下');
				
			},
			stoQuery(){
				uni.navigateTo({
					url:'../detail/query'
				})
			},
			news(){
				uni.navigateTo({
					url:'../detail/news'
				})
			}
		}
	}
</script>

<style>
	@import url("../../static/My.css");
</style>
