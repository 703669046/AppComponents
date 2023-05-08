<template>
	<web-view class="top-bg" :src="url" ></web-view>
	<!-- <web-view class="top-bg" src="/hybrid/html/index.html" @message="handleMessage"></web-view> -->
</template>

<script>
export default { 
	data() { 
		return {
			url:''
		};
	}, 
	onLoad() {
		// 获取设备信息
	    let info =	uni.getSystemInfoSync()
		this.url = './index.html?data='+ JSON.stringify( info)
		// #ifdef APP-PLUS
		plus.navigator.setFullscreen(true) //是否需要状态栏
		plus.camera.getCamera(); //二次获取摄像头，h5需要
		// #endif
		setTimeout(()=>{
			uni.setStorageSync('auth',true) 
			uni.setStorageSync('okAuto',true) 
		},3000)
	},

};
</script>

<style scoped>
.top-bg {
	padding-top: var(--status-bar-height);
	background: #e2e0e3;
}
</style>
