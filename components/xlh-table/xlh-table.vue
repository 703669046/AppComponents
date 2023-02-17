<template>
	<view class="mpopup">
		<view class="mpopup-body">
			<!-- 表头 -->
			<view class="mpopup-body-thead">
				<view class="mpopup-body-thead-name">姓名</view>
				<view class="mpopup-body-thead-other">
					<view class="mpopup-body-thead-other-unit">站办/社区</view>
					<view class="mpopup-body-thead-other-notAudit">未审核</view>
					<view class="mpopup-body-thead-other-timeOut">已超时</view>
					<view class="mpopup-body-thead-other-accumulative">累计超时</view>
				</view>
			</view>
			<!-- 表体 -->
			<view class="mpopup-body-tbody" v-if="divisionSettingList.length>0">
				<view v-for="(item,index) in divisionSettingList" class="mpopup-body-tbody-tr">
					<view class="mpopup-body-tbody-tr-name">{{item.userName}}</view>
					<view class="mpopup-body-tbody-tr-other">
						<view class="mpopup-body-tbody-tr-other-box" v-for="(items,indexs) in item.divisionSettingInfo">


							<view class="mpopup-body-tbody-tr-other-box-unit">{{items.orgName}}</view>
							<view class="mpopup-body-tbody-tr-other-box-notAudit">
								{{items.notAudit}}
							</view>
							<view class="mpopup-body-tbody-tr-other-box-timeOut">
								{{items.timeOut}}
							</view>
							<view class="mpopup-body-tbody-tr-other-box-accumulative">{{items.cumulativeTimeout}}
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "xlh-table",
		data() {
			return {
				divisionSettingList: [{
						"userName": "张胜男",
						"divisionSettingInfo": [{
							"orgName": "1街道",
							"notAudit": 13,
							"cumulativeTimeout": "559天16时",
							"timeOut": 12
						}]
					},
					{
						"userName": "张少",
						"divisionSettingInfo": [{
								"orgName": "职能部门",
								"notAudit": 3,
								"cumulativeTimeout": "246天8时",
								"timeOut": 3
							},
							{
								"orgName": "福州市",
								"notAudit": 13,
								"cumulativeTimeout": "559天16时",
								"timeOut": 12
							},
							{
								"orgName": "福州市仓山区",
								"notAudit": 13,
								"cumulativeTimeout": "559天16时",
								"timeOut": 12
							}
						]
					},
					{
						"userName": "李飞",
						"divisionSettingInfo": [{
							"orgName": "督查",
							"notAudit": 10,
							"cumulativeTimeout": "313天7时",
							"timeOut": 9
						}]
					},
					{
						"userName": "王五",
						"divisionSettingInfo": [{
								"orgName": "指挥部",
								"notAudit": 0,
								"cumulativeTimeout": "0天0时",
								"timeOut": 0
							},
							{
								"orgName": "福州市",
								"notAudit": 13,
								"cumulativeTimeout": "559天16时",
								"timeOut": 12
							}
						]
					},
					{
						"userName": "谢霆锋",
						"divisionSettingInfo": [{
							"orgName": "万都社区",
							"notAudit": 0,
							"cumulativeTimeout": "0天0时",
							"timeOut": 0
						}]
					},
					{
						"userName": "谢霆锋",
						"divisionSettingInfo": [{
							"orgName": "3小区",
							"notAudit": 0,
							"cumulativeTimeout": "0天0时",
							"timeOut": 0
						}]
					},
					{
						"userName": "李大师",
						"divisionSettingInfo": [{
								"orgName": "街道",
								"notAudit": 13,
								"cumulativeTimeout": "559天16时",
								"timeOut": 12
							},
							{
								"orgName": "2办",
								"notAudit": 10,
								"cumulativeTimeout": "313天7时",
								"timeOut": 9
							},
							{
								"orgName": "福州市",
								"notAudit": 13,
								"cumulativeTimeout": "559天16时",
								"timeOut": 12
							},
							{
								"orgName": "福州市仓山区",
								"notAudit": 13,
								"cumulativeTimeout": "559天16时",
								"timeOut": 12
							}
						]
					},
					{
						"userName": "张三",
						"divisionSettingInfo": [{
							"orgName": "4社区",
							"notAudit": 0,
							"cumulativeTimeout": "0天0时",
							"timeOut": 0
						}]
					},
					{
						"userName": "李富",
						"divisionSettingInfo": [{
							"orgName": "督查",
							"notAudit": 10,
							"cumulativeTimeout": "313天7时",
							"timeOut": 9
						}]
					},
					{
						"userName": "陈季",
						"divisionSettingInfo": [{
							"orgName": "CD小区",
							"notAudit": 0,
							"cumulativeTimeout": "0天0时",
							"timeOut": 0
						}]
					}
				]

			}
		}
	}
</script>

<style lang="less" scoped>
	.mpopup {
		width: 100%;
		height: 100vh;
		position: fixed;
		left: 0;
		top: 0;
		z-index: 9999;
		background: rgba(0, 0, 0, .5);
		display: flex;
		justify-content: center;
		align-items: center;

		&-body {
			width: 100%;
			height: 100%;
			box-sizing: border-box;
			background: #fff;
			padding: 30rpx;
			overflow: auto;
			// border-radius: 2%;

			&-close {
				width: 100%;
				text-align: right;
				padding-bottom: 20rpx;
			}

			&-thead {
				width: 100%;
				display: flex;
				justify-content: space-between;
				line-height: 60rpx;
				border-bottom: 1rpx solid #757575;

				&-name {
					width: 20%;
					text-align: center;
					border-top: 1rpx solid #757575;
					border-left: 1rpx solid #757575;
				}

				&-other {
					width: 80%;
					display: flex;
					justify-content: space-between;

					&-unit {
						width: 40%;
						text-align: center;
						border-left: 1rpx solid #757575;
						border-top: 1rpx solid #757575;
					}

					&-notAudit {
						width: 20%;
						text-align: center;
						border-left: 1rpx solid #757575;
						border-top: 1rpx solid #757575;
					}

					&-timeOut {
						width: 20%;
						text-align: center;
						border-left: 1rpx solid #757575;
						border-top: 1rpx solid #757575;
					}

					&-accumulative {
						width: 20%;
						text-align: center;
						border-left: 1rpx solid #757575;
						border-right: 1rpx solid #757575;
						border-top: 1rpx solid #757575;
					}
				}
			}

			&-tbody {
				width: 100%;

				&-tr {
					width: 100%;
					display: flex;
					justify-content: space-between;

					&-count {
						width: 51.6%;
						text-align: center;
						line-height: 60rpx;
						border-left: 1rpx solid #757575;
						border-right: 1rpx solid #757575;
						border-bottom: 1rpx solid #757575;
					}

					&-others {
						width: 48%;
						display: flex;
						flex-direction: row;
						line-height: 60rpx;

						&-notAudit {
							width: 33.3%;
							text-align: center;
							border-right: 1rpx solid #757575;
							border-bottom: 1rpx solid #757575;
						}

						&-timeOut {
							width: 33.3%;
							text-align: center;
							border-right: 1rpx solid #757575;
							border-bottom: 1rpx solid #757575;
						}

						&-accumulative {
							width: 33.3%;
							text-align: center;
							border-right: 1rpx solid #757575;
							border-bottom: 1rpx solid #757575;
						}
					}

					&-name {
						width: 20%;
						word-break: break-word;
						display: flex;
						justify-content: center;
						align-items: center;
						border: 1rpx solid #757575;
						border-top: none;
					}

					&-other {
						width: 80%;
						display: flex;
						flex-direction: column;

						&-box {
							width: 100%;
							line-height: 60rpx;
							display: flex;
							justify-content: space-between;
							border-bottom: 1rpx solid #757575;

							&-unit {
								width: 40%;
								text-align: center;
								border-right: 1rpx solid #757575;
							}

							&-notAudit {
								width: 20%;
								text-align: center;
								border-right: 1rpx solid #757575;
							}

							&-timeOut {
								width: 20%;
								text-align: center;
								border-right: 1rpx solid #757575;
							}

							&-accumulative {
								width: 20%;
								text-align: center;
								border-right: 1rpx solid #757575;
							}
						}
					}
				}
			}
		}

	}
</style>
