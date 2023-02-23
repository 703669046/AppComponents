<template>
	<view>
		<view style="text-align: center;margin-bottom: 20rpx;">
			已配对蓝牙设备
		</view>
		<view style="text-align: center;padding: 10rpx 0;border: 1rpx solid #2B85E4;border-radius: 15rpx;" class="uni-flex uni-row" v-for="(device,index) in deviceList" :key="index">
			<view style="width: 30rpx;">{{index+1}}</view>
			<view style="width: 180rpx;">{{device.name}}</view>
			<view style="width: 310rpx;">{{device.address}}</view>
			<view class="flex-item"><button type="primary" plain size="mini" @click="printSomething(device,printTest)">打印测试</button></view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				show: {
					setting: false
				},
				deviceList: []
			}
		},
		onLoad(p) {
			var that = this;
			// #ifdef APP-PLUS
			that.initPrinter();
			// #endif
		},
		methods: {
			showPrinterList: function() {
				var that = this;
				that.show.setting = true;
				for (var i = 0; i < that.deviceList.length; i++) {
					if (that.deviceList[i].name == that.device.name) {
						that.deviceList[i].checked = true
					}
				}
			},
			initPrinter: function() {
				var that = this;
				that.deviceList = [];
				var main = plus.android.runtimeMainActivity();
				var Context = plus.android.importClass("android.content.Context");
				var BManager = main.getSystemService(Context.BLUETOOTH_SERVICE);
				plus.android.importClass(BManager); 
				var BAdapter = BManager.getAdapter();
				plus.android.importClass(BAdapter); 
				var lists = BAdapter.getBondedDevices();
				plus.android.importClass(lists);
				var iterator = lists.iterator();
				plus.android.importClass(iterator);
				while (iterator.hasNext()) {
					var d = iterator.next();
					plus.android.importClass(d);
					var temp = {
						name: d.getName(),
						address: d.getAddress(),
						status: d.getBondState(),
						uuids: d.getUuids(),
						op: d
					};
					that.deviceList.push(temp);
				}
			},
			printSomething: function(dev,sb) {
				var that = this;
				var main = plus.android.runtimeMainActivity();
				var BluetoothAdapter = plus.android.importClass("android.bluetooth.BluetoothAdapter");
				var UUID = plus.android.importClass("java.util.UUID");
				var uuid = UUID.fromString("00001101-0000-1000-8000-00805F9B34FB");
				var BAdapter = BluetoothAdapter.getDefaultAdapter();
				var device = BAdapter.getRemoteDevice(dev.address);
				plus.android.importClass(device);
				var bluetoothSocket = device.createInsecureRfcommSocketToServiceRecord(uuid);
				plus.android.importClass(bluetoothSocket);
				console.log("开始连接打印机:"+dev.name);
				if (!bluetoothSocket.isConnected()) {
					bluetoothSocket.connect();
					if (bluetoothSocket.isConnected()) {
						console.log("设备已连接,开始发送打印文件");
						var outputStream = bluetoothSocket.getOutputStream();
						plus.android.importClass(outputStream);
						sb(outputStream);
						bluetoothSocket.close();
						if (!bluetoothSocket.isConnected()) {
							console.log("设备已关闭");
						}
					} else {
						uni.showToast({
						    title: '设备连接失败',
							icon:'error',
						    duration: 2000
						});
					}
				}
			},
			printTest: function(outputStream) {
				var that = this;
				var text = "! 0 200 200 840 1\r\n";
				text += "SPEED 1\r\n";
				//text += "VT 0 0 0 200 SF7444435088888\r\n";
				// text += "VT 0 0 540 200 SF7444435088888\r\n";
				// text += "VT 0 0 0 700 SF7444435088888\r\n";
				// text += "VT 0 0 540 700 SF7444435088888\r\n";
				
				//二维码
				text += "B QR 300 500 M 2 U 5\r\n";
				text += "MA,QR MMM={'k1':'023WA','k2':'023CA','k3':'005','k4':'T4','k5':'SF7444435088888','k6':'','k7':'51ba5363'}\r\n";
				text += "ENDQR\r\n";
				//时效类型
				//text += "RIGHT\r\n";
				text += "SETBOLD 1\r\n";
				text += "T 5 1 380 0 标快\r\n";
				text += "SETBOLD 0\r\n";
				//打印时间
				text += "CENTER\r\n";
				text += "T 0 0 0 10 2021-10-20 08:08:08\r\n";
				//一维码
				text += "BT OFF\r\n";
				text += "B 128 2 2 120 0 50 SF7444435088888\r\n";
				
				//子母单类型序号
				text += "T 3 0 0 180 1/1 母单 SF7444435088888\r\n";
				//打印边框
				text +=that.printBox({x:0,y:215},530,560,2,{t:true,l:true,b:true,r:true});
				//目的地城市代码
				text += "CENTER\r\n";
				text += "SETBOLD 1\r\n";
				text += "T 4 1 0 230 023CA-005\r\n";
				text += "SETBOLD 0\r\n";
				//地址
				text += "LEFT\r\n";
				text += "SETBOLD 1\r\n";
				text += "T 5 1 30 330 收\r\n";
				text += "SETBOLD 0\r\n";
				text += that.cutLine({
					x: 90,
					y: 330
				}, "北京市海淀区四海一家122号/王治业/15006666076");
				//打印线条
				text+="L 0 460 560 460 2\r\n";
				text+="L 250 460 250 745 2\r\n";
				
				text+="L 0 530 250 530 2\r\n";
				text+="L 0 570 250 570 2\r\n";
				//左下角付款方式等信息
				text += "T 3 0 30 480 到付\r\n";
				text += "T 3 0 30 540 已验视\r\n";
				//text += "T 3 0 30 580 S\r\n";
				text+=that.printLineList({x:30,y:580},["TF128885"])
			    text += "FORM\r\n";
				text += "END\r\n";
				text += "PRINT\r\n"
				// console.log(text)
				var arrayBuffer = plus.android.invoke(text, 'getBytes', 'gbk');
				outputStream.write(arrayBuffer);
				outputStream.flush();
			},
			printBox: function(p, l, w, k, s) { //起点坐标、长高、宽、线宽、显示(上左下右)
				var text = "";
				if (s.t) {
					text = text.concat("L ", p.x, " ", p.y, " ", w, " ", p.y, " ", k, "\r\n");
				}
				if (s.l) {
					text = text.concat("L ", p.x, " ", p.y, " ", p.x, " ", p.y+l, " ", k, "\r\n");
				}
				if (s.b) {
					text = text.concat("L ", p.x, " ", p.y+l, " ", w, " ", p.y+l, " ", k, "\r\n");
				}
				if (s.r) {
					text = text.concat("L ", w, " ", p.y+l, " ", w, " ", p.y, " ", k, "\r\n");
				}
				return text;
			},
			cutLine: function(p, str) {
				var r = "";
				var max = 18;
				var n = parseInt(str.length / max);
				for (var i = 0; i < n; i++) {
					var temp = str.substr(i * max, max);
					r += "T 3 0 " + p.x + " " + (p.y + 40 * i) + " " + temp + "\r\n"
				}
				var w = str.substr(n * max);
				r += "T 3 0 " + p.x + " " + (p.y + 40 * n) + " " + w + "\r\n";
				return r;
			},
			printLineList: function(p, list) {
				var r = "";
				for (var i = 0; i < list.length&&i<5; i++) {
					r += "T 3 0 " + p.x + " " + (p.y + 40 * i) + " " + list[i] + "\r\n"
				}
				return r;
			},
			printLineList2: function(p, list) {
				var r = "";
				for (var i = 0; i < list.length&&i<10; i=i+2) {
					r += "T 3 0 " + p.x + " " + (p.y + 60 * i) + " " + list[i] + "\r\n";
					if((i+1)<list.length){
						r += "T 3 0 " + (p.x+150) + " " + (p.y + 60 * i) + " " + list[i+1] + "\r\n";
					}
				}
				return r;
			},
			mySleep:async function(time){
				await this.mypromise(time);
			},
			mypromise:function(time){
				return new Promise((resolve) => setTimeout(resolve, time));
			}
		}
	}
</script>

<style scoped>
	page{
		padding: 0;
		margin: 0;
	}
	.picked {
		background-color: lavender;
	}
</style>
