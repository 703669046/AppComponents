<template>
	<view class="page-content">
		<view class="containerV">
			<!-- 标题 -->
			<view class="headerV">
				<view class="top-tips1">
					<view>{{tipsText}}</view>
				</view>
			</view>
			<!-- 拍摄区域 -->
			<view class="contentV">
				<view class="mark"></view>
				<image class="image" v-if="tempImg" mode="widthFix" :src="tempImg" />
				<camera v-if='isAuthCamera' :device-position="devicePosition ?'front': 'back'" class="camera" flash="off" resolution='high' />
			</view>
			<!-- 操作区域 -->
			<view class="footerV">
				<view style="width: 100%;">
					<view v-if="!tempImg" style="width: 100%;">
						<view class="take-photo-bgV">
							<!-- 图片上传 -->
							<view v-show="true" class="btn-change-upload" @click="handleChooseImage" />
							<!--拍照-->
							<view class="btn-take-photo" @click="handleTakePhotoClick" />
							<!-- 切换镜头 -->
							<view class="btn-change-camera" @click="handleChangeCameraClick" />
						</view>
					</view>
					<view class="confirmV" v-else>
						<view class="btn-cancel" @click="handleCancelClick">
							取消
						</view>
						<view class="btn-ok" @click="handleOkClick">
							确定
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		components:{},
		data() {
			return {
				tipsText: '请将脸部移入框内', // 错误文案提示
				tempImg: '', // 本地图片路径
				cameraEngine: null, // 相机引擎
				devicePosition: true, // 摄像头朝向
				isAuthCamera: true, // 是否拥有相机权限
			};
		},onLoad() {
			this.init();
		},onShow() {
			
		},methods:{
			init(){ // 初始化相机引擎
			        plus.camera.PopPosition
					let cmr = plus.camera.getCamera() //获取相机对象
					    cmr.captureImage(
					        //调用拍照方法，获得临时路径
					        function (p) {
					            plus.io.resolveLocalFileSystemURL(p, function (entry) {
					                //通过临时路径，获得文件系统中的文件对象entry
					                entry.file(function (file) {
					                    // 可通过entry对象的file方法，获取文件数据对象（该文件数据对象仍无法直接使用）
					                    axios({
					                        method: 'get',
					                        url: entry.toRemoteURL(),
					                        responseType: 'blob',
					                    }).then(res => {
					                        let blob = res.data
					                        const uploadFile = new FormData()
					            			uploadFile.append('file', blob )
					            			axios({
							                    method: 'post',
							                    url: '/file/api/Upload',
							                    headers: { 'Content-Type': 'multipart/form-data'},
							                    data: uploadFile,
							                })
					                    })
					                    file.close()
					                })
					            })
					        },
					        function (error) {
					            console.log('---' + 'Capture image failed: ' + error.message)
					        },
					    )

			},
			handleChangeCameraClick() {  // 切换设备镜头
				this.devicePosition = !this.devicePosition;
			},
			handleChooseImage() {  	// 图片上传
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: (res) => {
						if (res.errMsg === 'chooseImage:ok') {
							this.handleOkClick(res.tempFilePaths[0])
						}
					},
					fail: (res) => {

					},
				});
			},
			handleTakePhotoClick() { // 拍照点击
				if (this.tipsText != "" && this.tipsText != "请拍照") {
					return;
				}
				uni.getSetting({
					success: (res) => {
						if (!res.authSetting['scope.camera']) {
							this.isAuthCamera = false
							uni.openSetting({
								success: (res) => {
									if (res.authSetting['scope.camera']) {
										this.isAuthCamera = true;
									}
								}
							})
						}
					}
				})
				this.cameraEngine.takePhoto({
					quality: "high",
					success: ({
						tempImagePath
					}) => {
						this.handleOkClick(tempImagePath)
					}
				})
			},
			async handleOkClick(filePath) {  // 点击确定上传
				uni.showLoading({title: '上传中...',mask: true})
				let img = await this.$util.uploadopt.image(filePath,2)
				uni.$emit('onface',img);
				uni.navigateBack();
			},
			handleCancelClick() {  // 点击 - 取消上传
				this.tempImg = ''
			},
		}
	}
</script>

<style lang="scss">
	page {height: 100%;width: 100%;}
	.page-content {
		width: 100%;height: 100%;
		.containerV {
			width: 100%;height: 100%;display: flex;flex-direction: column;
			/* 标题 */
			.headerV {
				padding: 163rpx 0 56rpx;text-align: center;
				.top-tips1 {color: #333333;font-size: 42rpx;}
			}
			/* 拍摄区域 */
			.contentV {
				position: relative;display: flex;flex-direction: column;align-items: center;justify-content: center;
				width: 564rpx;height: 564rpx;border-radius: 50%;margin: 0 auto;border: 10rpx solid #1568E0;background-color: #d8d8d8;
				.camera {width: 100%;height: 100%;border-radius: 50%;z-index: 2;}
				.mark {position: absolute;left: 0;top: 0;z-index: 1;width: 100%;height: 100%;border-radius: 50%;}
				.image {position: absolute;width: 100%;height: 100%;z-index: 3;border-radius: 50%;}
			}

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
					.btn-change-camera {right: 130rpx;width: 80rpx;height: 80rpx;background: url("https://www.hujinqiang.com/anjiantong/face/changing.png") no-repeat;background-size: 100% auto;}
				}
				.confirmV {
					margin: 200rpx 100rpx 0rpx 100rpx;display: flex;flex-direction: row;align-items: center;justify-content: space-between;
					.btn-cancel {font-size: 32rpx;color: #1C1C1C;}
					.btn-ok {font-size: 32rpx;color: #00AAFF;}
				}
			}
		}
	}
</style>

