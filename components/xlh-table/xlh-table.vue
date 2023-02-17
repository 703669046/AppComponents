<template>
	<view class="mpopup">
		<view class="mpopup-body">
			<!-- <view class="mpopup-body-close" @click="showPopup=false">关闭</view> -->
			<view class="mpopup-body-thead">
				<view class="mpopup-body-thead-name">姓名</view>
				<view class="mpopup-body-thead-other">
					<view class="mpopup-body-thead-other-unit">站办/社区</view>
					<view class="mpopup-body-thead-other-notAudit">未审核</view>
					<view class="mpopup-body-thead-other-timeOut">已超时</view>
					<view class="mpopup-body-thead-other-accumulative">累计超时</view>
				</view>
			</view>

			<view class="mpopup-body-tbody" v-if="listObj.divisionSettingList.length>0">
				<view class="mpopup-body-tbody-tr">
					<view class="mpopup-body-tbody-tr-count">总计</view>
					<view class="mpopup-body-tbody-tr-others">
						<view class="mpopup-body-tbody-tr-others-notAudit" @click.stop="handleNavigateTo(1)">
							{{listObj.notAuditTotal}}
						</view>
						<view class="mpopup-body-tbody-tr-others-timeOut" @click.stop="handleNavigateTo(2)">
							{{listObj.timeOutTotal}}
						</view>
						<view class="mpopup-body-tbody-tr-others-accumulative">{{listObj.cumulativeTimeoutTotal}}
						</view>

					</view>
				</view>
				<view v-for="(item,index) in listObj.divisionSettingList" class="mpopup-body-tbody-tr">
					<view class="mpopup-body-tbody-tr-name">{{item.userName}}</view>
					<view class="mpopup-body-tbody-tr-other">
						<view class="mpopup-body-tbody-tr-other-box" v-for="(items,indexs) in item.divisionSettingInfo">


							<view class="mpopup-body-tbody-tr-other-box-unit">{{items.orgName}}</view>
							<view class="mpopup-body-tbody-tr-other-box-notAudit" @click.stop="handleNavigateTo(1)">
								{{items.notAudit}}
							</view>
							<view class="mpopup-body-tbody-tr-other-box-timeOut" @click.stop="handleNavigateTo(2)">
								{{items.timeOut}}
							</view>
							<view class="mpopup-body-tbody-tr-other-box-accumulative">{{items.cumulativeTimeout}}
							</view>
						</view>
					</view>
				</view>
			</view>
			<uni-page :total="total" :pageSize="param.limit" @change="changePage"></uni-page>
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

<style>

</style>
