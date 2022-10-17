<template>
	<view>
	<view class="phoneBack col">
		<text class="phoneTit textColor">手机号码归属地专业在线查询</text>
	</view>	
	<view class="row">
	<view class="row">
		<input class="phoneIn" maxlength=7 v-on:blur="type" placeholder-style="color:#c0c0c0" placeholder="请输入手机号码前七位"/>
	</view>
	<view class="row">
		<button v-on:click="query" type="primary">查询</button>
	</view>
	</view>
	<view class="place4"></view>
	<view class="row">
		<view class="place3 col"><text>手机号</text></view>
		<view class="place3 col"><text>{{number+"****"}}</text></view>
	</view>
	<view class="row">
		<view class="place3 col"><text>归属地</text></view>
		<view class="place3 col"><text>{{place[0]+"省 "+place[1]+"市"}}</text></view>
	</view>
	<view class="row">
		<view class="place3 col"><text>卡类型</text></view>
	    <view class="place3 col"><text>{{place[2]+"卡"}}</text></view>
	</view>
	<view class="row">
		<view class="place3 col"><text>区号</text></view>
		<view class="place3 col"><text>{{place[3]}}</text></view>
	</view>
	<view>
		<image src="../../static/phone/5g.png" mode="heightFix"></image>
	</view>
	</view>
</template>

<script>
	let number;
	let place;
	export default{
		data(){
			return {
				place:['**','**','***','****'],
				number:''
				}
			
		}	,	
		
		methods:{
				onLoad() {
					uni.showLoading({
					    title: '加载中'
					})
					setTimeout(()=>{uni.hideLoading()()},700)
				},
			type(event){
				number=event.detail.value;
				
			},
			query(){
				uni.showLoading({
				    title: '查询中'
				});
				uni.request({
					url:'http://apis.juhe.cn/mobile/get',
					data:{
						key : 'c0328ef77137359664a1f18630ae3854',
						phone:number
					},
					
					success(res) {
						console.log(res)
						if(res.data.reason=="Return Successd!"){
						uni.setStorageSync('place',[res.data.result.province,res.data.result.city,res.data.result.company,res.data.result.areacode])
					}
					else{}
					}
				}),
				
				setTimeout(()=>{
				this.place=uni.getStorageSync('place')
				uni.hideLoading();
				},1800
				)
			}
		}
	}
</script>

<style>
	@import url("../../static/My.css");

</style>