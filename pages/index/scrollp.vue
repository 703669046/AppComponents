<template>
	<view class="container">

		<view class="header-box">
			<view class="header-box-title">最近办结</view>
			<view class="header-box-subTitle">(你呼我应)</view>
		</view>
		<view class="body-list">
			<view class="body-list-item" v-for="(item,index) in listData" :key="index">
				<view class="list-item body-list-item-title">{{item.problemDescription}}</view>
				<view class="list-item body-list-item-category">类别：{{其他工作}}</view>
				<view class="list-item body-list-item-source">上报来源：第一执法队</view>
				<view class="list-item body-list-item-bottom">
					<view class="body-list-item-bottom-time">上报时间：{{item.inputDate | getDate}}</view>
					<view class="body-list-item-bottom-static">{{item.states | getStatic}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				param: {
					limit: 10,
					start: 0
				},
				total: 0,
				listData: []
			}
		},
		filters: {
			getDate(val) {
				const timeFormat = (type, time) => {
					if (type == "UTC" || type == "DateNow") {
						let tmpTime = new Date(time);
						return formatTimIng(tmpTime);
					} else {
						return formatTimIng(time);
					}
				};
				// 格式化中
				const formatTimIng = (time) => {
					let year = time.getFullYear();
					let month = time.getMonth() + 1 < 10 ? "0" + (time.getMonth() + 1) : time.getMonth() + 1;
					let day = time.getDate() + 1 < 10 ? "0" + (time.getDate() + 1) : time.getDate() + 1;
					let hours = time.getHours() + 1 < 10 ? "0" + (time.getHours() + 1) : time.getHours() + 1;
					let minutes = time.getMinutes() + 1 < 10 ? "0" + (time.getMinutes() + 1) : time.getMinutes();
					let seconds = time.getSeconds() + 1 < 10 ? "0" + (time.getSeconds() + 1) : time.getSeconds();
					return (
						year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds
					);
				};
				return timeFormat("UTC", val),
			},
			getStatic(val) {
				switch (val) {
					case "0":
						return '待办理'
						break;
					case "1":
						return '办理中'
						break;
					case "2":
						return '已归档'
						break;
					case "3":
						return '已拒绝'
						break;
					case "4":
						return '已办理'
						break;
					case "5":
						return '已分派'
				}
			}
		},
		mounted() {
			this.initData();
		},
		methods: {
			initData() {
				let self = this;
				LsxmRequest.request({
					url: "/uni/civil/publicEventsPage",
					method: 'POST',
					data: this.param
				}).then(function(result) {
					self.listData = result.list;
					self.total = result.total;
				})
			}
		}
	}
</script>

<style>
	.container {
		padding: 20rpx;
		font-size: 14rpx;
		line-height: 24rpx;
	}

	.header-box {
		width: 100%;
		display: flex;
		flex-direction: row;
		border-left: 3rpx solid #FA855A;
		line-height: 30rpx;
		margin-bottom: 15rpx;
	}

	.header-box-title {
		padding: 0 10rpx 0 15rpx;
		font-size: 17rpx;
		font-weight: bold;
	}

	.header-box-subTitle {
		font-size: 14rpx;
	}


	.body-list {
		padding-left: 15rpx;
	}

	.body-list-item {}

	.list-item {
		line-height: 25rpx;
		color: #858585;
		font-size: 12rpx
	}

	.body-list-item-title {
		color: #000;
		font-size: 14rpx;
	}

	/**/
	.body-list-item-bottom {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.body-list-item-bottom-static {
		margin-right: 15rpx;
	}
</style>
