<template>
	<view>
		<uni-forms ref="baseForm" >
			<uni-forms-item label="城市" required>
				<picker class="addRess" @change="bindPickerChange" @columnchange="pluginclass" :value="pickVal" :range="cityArr"
					range-key="AreaName" mode="multiSelector">
					<view v-if="pickVal2.length==0">请选择</view>
					<view v-else class="uni-input">
						<text>{{cityArr[0][pickVal2[0]].AreaName}}</text>
						<text>{{cityArr[1][pickVal2[1]].AreaName}}</text>
						<text>{{cityArr[2][pickVal2[2]].AreaName}}</text>
					</view>
				</picker>
			</uni-forms-item>
		</uni-forms>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pickVal: [0, 0, 0], // 选择器默认值
				pickVal2:[],   // 辅助城市名字线索
				cityArr: [],   // 所有城市
			}
		},
		mounted() {
			// 页面初始化
			this.loadProvinces()
		},
		methods: {
			loadProvinces() { // 加载省份
				uni.request({
					url: 'http://test-api.tiananhub.com/api/province/GetListProvince',
					method: 'get',
					success: async (res) => {
						let {
							data
						} = res.data
						console.log(data)
						this.cityArr[0] = data
						this.loadCities(data[0].AreaId)
						this.$forceUpdate()
					},
					fail: async (res) => {}
				})
			},
			// 请求地级市
			loadCities(AreaId) {
				uni.request({
					url: 'http://test-api.tiananhub.com/api/province/GetListCity',
					data: {
						AreaId
					},
					method: 'get',
					success: async (res) => {
						let {
							data
						} = res.data
						this.cityArr[1] = data
						this.loadAreas(data[0].AreaId)

						this.$forceUpdate()
					},
					fail: async (res) => {}
				})
			},
			// 请求县区市
			loadAreas(AreaId) {
				uni.request({
					url: 'http://test-api.tiananhub.com/api/province/GetListCity',
					data: {
						AreaId
					},
					method: 'get',
					success: async (res) => {
						let {
							data
						} = res.data
						this.cityArr[2] = data

						this.$forceUpdate()
					},
					fail: async (res) => {}
				})
			},
			// 确定其他选择的值
			bindPickerChange(data) {
				this.pickVal=data.target.value;
				this.pickVal2=data.target.value;
				// 以下拓展区域业务逻辑
			},
			// 选择时且未点击确定是的值
			pluginclass(e) {
				console.log('修改的列为：' + e.detail.column + '，值为：' + e.detail.value)
				if (e.detail.column == 0) {
					this.loadCities(this.cityArr[0][e.detail.value].AreaId);
				}
				if (e.detail.column == 1) {
					this.loadAreas(this.cityArr[1][e.detail.value].AreaId);
				}
			}
		}
	}
</script>
<style lang="scss" scoped>
	.addRess{
		line-height: 80rpx;
	}
</style>
