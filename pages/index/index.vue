<template>
	<view class="container">
		
		<uni-list-item title="蓝牙" to='../' @click="navigateToSession('/components/xlh-bluetooth/xlh-bluetooth')"
			showArrow />
		<uni-list-item title="地址" to="../" @click="navigateToSession('/pages/xlh-picker/xlh-picker')" showArrow />
		<button type="primary" @click="handleNavigateTo()">table</button> 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				href: 'https://uniapp.dcloud.io/component/README?id=uniui'
			}
		},
		methods: {
			navigateToSession(url) {
				uni.navigateTo({
					url: url
				});
			},
			handleNavigateTo(){
				uni.navigateTo({
					url:'/pages/table/table'
				})
			}
		}
	}
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>
