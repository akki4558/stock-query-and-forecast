<template>
	<view>
		<image class="news" src="../../static/news/n1.png" mode="scaleToFill"></image>
		<image class="news" src="../../static/news/n2.png" mode="scaleToFill"></image>
		<image class="news" src="../../static/news/n3.png" mode="scaleToFill"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
			uni.showLoading({
			    title: '加载中'
			})
			setTimeout(()=>{uni.hideLoading()()},700)
		},
		methods: {
			
		}
	}
</script>

<style>
@import url("../../static/My.css");
</style>
