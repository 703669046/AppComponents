<template>
	<view class="pages-box">
		<view class="tpls">将身份证（{{opposite==1?'人像面':'国徽面'}}）放在此区域，进行拍照上传</view>
		<view class="camera-box">
			<camera device-position="back" flash="auto" @error="error" style="width: 100%; height: 500upx;">
				<cover-image src="https://www.hujinqiang.com/anjiantong/face/scan-img.png" class="scan-img"></cover-image>
			</camera>
		</view>
		<view class="scan-text">拍摄要求：清晰完整，避免缺边、模糊、反光。</view>
		<view class="footerV">
			<view class="take-photo-bgV">
				<!--返回 -->
				<view class="btn-change-back" @click="handleBaCK" />
				<!--拍照-->
				<view class="btn-take-photo" @click="handleTakePhotoClick" />
				<!--图片上传 -->
				<view class="btn-change-upload" @click="handleChooseImage" />
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		components:{},
		data() {
			return {
				src:'',
				opposite:1,
			};
		},
		onLoad(e) {
			console.log(e)
			if(e && e.opposite){
				this.opposite = e.opposite=='1'?1:2;
				let _title = e.opposite=='1'?'身份证正面识别':'身份证反面识别'
				uni.setNavigationBarTitle({title:_title})
			}
		},
		methods: {
			error(e) {
				console.log(e.detail);
			},
			handleBaCK(){ //返回
				uni.navigateBack({})
			},
			handleTakePhotoClick() {   //拍照
				let that = this;
				const ctx = uni.createCameraContext();
				ctx.takePhoto({
					quality: 'high',
					success: (res) => {
						that.handleOkClick(res.tempImagePath)
						/* 返回调用页面并把图片URL传递过去 */
						/* let pages = getCurrentPages();
						let prevPage = pages[pages.length - 2]; 
						prevPage.setData({
							"image": res.tempImagePath,
						})
						uni.navigateBack(); */
						
						/* 调用页面获取图片URL方法 */
						/* let pages = getCurrentPages();
						let currPage = pages[pages.length-1];
						if(typeof(currPage.data.image) != undefined && currPage.data.image != null){
							console.log('获取图片：', currPage.data.image)
						} */
					}
				});
			},
			handleChooseImage() {  	// 图片上传
				let that = this;
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: (res) => {
						if (res.errMsg === 'chooseImage:ok') {
							that.handleOkClick(res.tempFilePaths[0])
						}
					},
					fail: (res) => {
			
					},
				});
			},
			async handleOkClick(filePath) {  // 点击确定上传
				uni.showLoading({title: '上传中...',mask: true})
				let img = await this.$util.uploadopt.image(filePath,2)
				let isStr = this.opposite==1?true:false;
				uni.$emit('onucard',isStr,img)
				uni.navigateBack();
			},
		}
	}
</script>

<style lang="scss">
	page{width: 100%;height: 100%;}
	.pages-box{
		display: flex;flex-direction: column;align-items: center;justify-content: center;
		width: 100%;height: 100%;
		.tpls{font-size: 28rpx;text-align: center;padding: 80rpx 0 40rpx;}
		.camera-box{
			width: 750rpx;height: 500rpx;
			.scan-img{opacity: 0.4;width: 100%;height:500upx;}
		}
		.scan-text{font-size: 28rpx;text-align: center;padding: 30rpx 0 30rpx;}
		/* 操作按钮 */
		.footerV {
			width: 100%;flex-grow: 1;display: flex;flex-direction: row;align-items: center;justify-content: center;
			.privacyV {
				padding-top: 30rpx;display: flex;flex-direction: row;align-items: center;justify-content: center;
				.text {
					font-size: 30rpx;color: #1C1C1C;text-align: center;line-height: 42rpx;margin-left: 15rpx;
					text {font-size: 30rpx;color: #00AAFF;text-align: center;line-height: 42rpx;}
				}
			}
			.bottom-tips-2 {margin-top: 20rpx;color: #999999;text-align: center;font-size: 26rpx;}
		
			.take-photo-bgV {
				width: 100%;margin-top: 30rpx;display: flex;flex-direction: row;align-items: center;justify-content: center;
				// 由于左边没有按钮，所以左边要便宜更大，以便是拍照按钮居中
				.btn-take-photo {margin: 0rpx 80rpx 0rpx 80rpx;width: 196rpx;height: 196rpx;background: url("https://www.hujinqiang.com/anjiantong/face/photo.png") no-repeat;background-size: 100% auto;}
				.btn-change-upload {left: 130rpx;width: 80rpx;height: 80rpx;background: url("https://www.hujinqiang.com/anjiantong/face/album.png") no-repeat;background-size: 100% auto;}
				.btn-change-back {right: 130rpx;width: 80rpx;height: 80rpx;background: url("https://www.hujinqiang.com/anjiantong/face/back.png") no-repeat;background-size: 100% auto;}
			}
			.confirmV {
				margin: 200rpx 100rpx 0rpx 100rpx;display: flex;flex-direction: row;align-items: center;justify-content: space-between;
				.btn-cancel {font-size: 32rpx;color: #1C1C1C;}
				.btn-ok {font-size: 32rpx;color: #00AAFF;}
			}
		}
	}
	
	
	
	
</style>
