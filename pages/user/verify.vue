<template>
    <view class="content" v-if="showHeader"
        style="position: fixed;top: 0;left: 0;z-index: 777;width: 100%;height: 100vh;background-color: #FFFFFF;">
        <camera :binderror="handleCameraError" :device-position="devicePosition" flash="off"
            style="width: 100%; height: 80vh;">
            <cover-image src="https://img-blog.csdnimg.cn/20210126152753150.png"
                style="width: 100%;height: 700rpx;margin-top:100rpx;"></cover-image>

        </camera>
        <view class="btns" style="width: 100%;height: 20vh;background: #3B4144;">
            <image class="item" @tap="chooseImage" src="../../static/photo.png"></image>
            <image class="item" @tap="takePhotoByHead" src="../../static/photo.png"></image>
            <image class="item" @tap="reverseCamera" src="../../static/photo.png"></image>
        </view>
        <!-- <view class="error-handler" v-if="!authCamera">
            <button class="nobtn" openType="openSetting">打开相机授权</button>
        </view> -->
    </view>
</template>

<script>
    export default {
        data() {
            return {
                authCamera: false,
                showHeader: true,
                ctxHeader: null,
                devicePosition:'front'
            }
        }, 
        watch:{
            showHeader(val){
                console.log(val)
                return
                var that = this;
                //获取相机权限
                uni.getSetting({
                    success(res) {
                        console.log('相机权限:', res)
                        if (res.authSetting["scope.camera"]) {
                            that.authCamera = true
                        } else {
                            that.authCamera = false
                            // uni.showModal({
                            //  title: '权限申请',
                            //  content: '需要摄像头权限，以拍摄照片。',
                            //  success: (btn_res) => {
                            //      if (btn_res.confirm) {
                            //          uni.authorize({
                            //              scope: 'scope.camera',
                            //              success() {
                            //                  that.authCamera = true
                            //              }
                            //          })
                            //      } else if (btn_res.cancel) {

                            //      }
                            //  }
                            // });
                        }
                    }
                })
            }
        },
        onShow() {
            var that = this;
            //获取相机权限
            uni.getSetting({
                success(res) {
                    console.log('相机权限:', res)
                    if (res.authSetting["scope.camera"]) {
                        that.authCamera = true
                    } else {
                        that.authCamera = false
                        // uni.showModal({
                        //  title: '权限申请',
                        //  content: '需要摄像头权限，以拍摄照片。',
                        //  success: (btn_res) => {
                        //      if (btn_res.confirm) {
                        //          uni.authorize({
                        //              scope: 'scope.camera',
                        //              success() {
                        //                  that.authCamera = true
                        //              }
                        //          })
                        //      } else if (btn_res.cancel) {

                        //      }
                        //  }
                        // });
                    }
                }
            })
        },
        onLoad() {
            // console.log(window.innerHeight)
            // if (uni.createCameraContext) {
            //  setTimeout(() => {
            //      this.cameraContext = uni.createCameraContext();
            //  }, 200)
            // } else {
            //  // 如果希望用户在最新版本的客户端上体验您的小程序，可以这样子提示
            //  uni.showModal({
            //      title: '提示',
            //      content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后重试。'
            //  })
            // }
        },
        methods: {
            chooseImage(){
                uni.chooseImage({
                    count: 1, //默认9
                    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
                    sourceType: ['album'], //从相册选择
                    success: (res)=> {
                        this.newPath = res.tempFilePaths[0]
                        console.log(this.newPath)
                        this.$store.commit('set_photo', this.newPath)
                        uni.navigateBack({
                            delta: 1
                        });
                    }
                });
            },
            //拍摄头像
            takePhotoByHead() {
                this.showHeader = true //开启拍照
                this.ctxHeader = uni.createCameraContext();
                this.ctxHeader.takePhoto({
                    quality: 'high',
                    success: (res) => {
                        console.log(res)
                        uni.compressImage({
                            src: res.tempImagePath,
                            quality: 90, //压缩比例
                            success: ress => {
                                this.newPath = ress.tempFilePath; //图片
                                console.log(this.newPath)
                                this.$store.commit('set_photo', this.newPath)
                                uni.navigateBack({
                                    delta: 1
                                });
                            }
                        })


                    }
                });
            },
            handleCameraError() {
                uni.showToast({
                    title: '用户拒绝使用摄像头',
                    icon: 'none'
                })
            },
            reverseCamera() {
                this.devicePosition = (("back" === this.devicePosition) ? "front" : "back")
            },
        }
    }
</script>

<style lang="scss" scoped>
    .content {
        display: flex;
        flex-direction: column;
        // align-items: center;
        justify-content: center;
        background: #fff;
        box-sizing: border-box;
        height: 100%;
        width: 100vw;

        .btns {
            display: flex;
            justify-content: space-around;
            align-items: center;

            .item {
                width: 100rpx;
                height: 100rpx;
            }

        }
    }
</style>