<template>
	<view>
		<loading :loadModal="loadModal"></loading>
		<cu-custom bgColor="bg-gradual-blue" class="customHead" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">派工</block>
		</cu-custom>
		<uni-fab :pattern="pattern" :horizontal="horizontal" :vertical="vertical" cuIcon="add" :popMenu="popMenu"
			distable :direction="direction" @fabClick="fabClick"></uni-fab>
		<view class="box getheight">
			<view class="cu-bar bg-white solid-bottom" style="height: auto;">
				<view class="action">
					<text>单号:{{form.FBillNo}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom" style="height: auto;">
				<view class="action">
					<text>计划数量:{{form.FProcedurePlanNumber}}</text>
				</view>
				<view class="action">
					<text>生产数量:{{form.FProduceNumber}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom" style="height: auto;">
				<view class="action">
					<text>已派工数量:{{form.FWBSentNum}}</text>
				</view>
				<view class="action">
					<text>未派工数量:{{form.FWBNuSentNum}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom flex-wrap" style="height: auto;">
				<view class="action">
					<text>产品名称:{{form.FItemName}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom flex-wrap" style="height: auto;">
				<view class="action">
					<text>产品编码:{{form.FItemNumber}}</text>
				</view><view class="action">
					<text>班组:{{form.FTeamName}}</text>
				</view>

			</view>
			<view class="cu-bar bg-white solid-bottom" style="height: auto;">
				<view class="action">
					<text>规格型号:{{form.FModel}}</text>
				</view>
				<view class="action">
					<text>工序名称:{{form.FAlternateName}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom" style="height: auto;">
				<view class="action">
					<text>计划开工日期:{{form.FPlanStartDate}}</text>
				</view>
				<view class="action">
					<text>计划完工日期:{{form.FPlanFinishedDate}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom" style="height: 60upx;">
				<view class="action">
					<view class="title">加工说明:</view>
					<input name="input" @input='sumRemark' style="font-size: 13px;text-align: left;" v-model="form.fprocessnote" />
				</view>
			</view>
		</view>
		<view class="cu-modal" :class="modalName=='Modal'?'show':''">
			<view class="cu-dialog" style="height: 70%;margin-top: 20%;">
				<view class="cu-bar bg-white justify-end" style="height: 60upx;">
					<view class="content">人员信息</view>
					<view class="action" @tap="hideModal">
						<text class="cuIcon-close text-red"></text>
					</view>
				</view>
				<view style="height: 100%;overflow: auto;text-align: left;">
					<city-select @cityClick="cityClick" :formatName="formatName" :obtainCitys="empList" :isSearch="true"
						style="width: auto !important;" ref="citys"></city-select>
				</view>
			</view>
		</view>
		<scroll-view scroll-y class="page" :style="{ 'height': pageHeight + 'px' }">
			<!-- <view class="cu-bar bg-olive solid-bottom" style="height: 60upx;">
				<view class="action">
					<view style="width: 90px;">班组:</view>
					<ld-select :list="deptList" list-key="FName" value-key="FNumber" placeholder="请选择" clearable
						v-model="form.fdeptID" @change="deptChange"></ld-select>
				</view>
				<view class="action">
					<view style="width: 90px;">工序:</view>
					<ld-select :list="processList" list-key="FName" value-key="FNumber" placeholder="请选择" clearable
						v-model="form.processID" @change="processChange"></ld-select>
				</view>
			</view>
			<view class="cu-bar bg-olive solid-bottom" style="height: 60upx;">
			<view class="action">
				计划数量:<text>{{form.planNum}}</text>
			</view>
			<view class="action">
				开工日期:
				<ruiDatePicker
				    fields="day"
					class='ruidata'
				    start="2010-00-00"
				    end="2030-12-30"
					:value="form.workDate"
				    @change="bindChange"
				></ruiDatePicker>
			</view>
			<view class="action">
				合计:<text>{{form.bNum}}</text>
			</view>
		</view> -->
			<view v-for="(item,index) in cuIList" :key="index">
				<view class="cu-list menu-avatar">
					<view class="bg-white" style="width: 100%;margin-top: 2px;padding:15rpx 0 15rpx 0;height: auto;"
						:data-target="'move-box-' + index">
						<view style="clear: both;width: 100%;" class="grid text-center col-2">
							<!-- <view>
								<ld-select :list="deptList" list-key="FName" value-key="FNumber" placeholder="班组"
									clearable v-model="item.FDeptID" @change="deptChange"></ld-select>
							</view> -->
							<!-- <view >
								<ld-select :list="shiftList" list-key="FName" value-key="FNumber" placeholder="班次"
									clearable v-model="item.FClassNumber" @change="shiftChange($event, item)">
								</ld-select>
							</view> -->
							<!-- <view>
								<picker @change="PickerChange($event, item)" :value="pickerVal" :range-key="'FName'" :range="shiftList">
									<view class="picker">
										<button class="cu-btn sm round bg-blue shadow" >
										<text class="cuIcon-home">
										</text>班次:{{item.FClassName}}</button>
									</view>
								</picker>
							</view> -->
							<view>
								<button class="cu-btn sl round bg-green shadow" @tap="showModal(index, item)"
									:disabled="isDis" data-target="Modal">
									<text class="cuIcon-people">
									</text>操作人:{{item.userName}}</button>
							</view>
							<view>
								<view style="float: left;line-height: 60upx;">派工量:</view>
								<input name="input" type="number" style="border-bottom: 1px solid;"
									@input='sumCount($event, item)' v-model="item.FSendQty" />
							</view>
							<view style="width: 100%;">
								<button class="cu-btn sm round shadow bg-red" @tap="del(index,item)">删除</button>
							</view>
						</view>
						<!-- <view class="move">
							<view class="bg-red" @tap="del(index,item)">删除</view>
						</view> -->
					</view>
				</view>
			</view>
			<view class="cu-bar tabbar shadow foot">
				<view class="box text-center">
					<button :disabled="isClick" class="cu-btn bg-green shadow-blur round lg" style="width: 40%;"
						@tap="$manyCk(saveData)">提交</button>
					<!-- <button class="cu-btn bg-blue shadow-blur round lg" style="width: 40%;" @tap="$manyCk(clearList)">清空</button> -->
				</view>
			</view>
		</scroll-view>

	</view>
</template>
<script>
	import ruiDatePicker from '@/components/rattenking-dtpicker/rattenking-dtpicker.vue';
	import ldSelect from '@/components/ld-select/ld-select.vue'
	import uniFab from '@/components/uni-fab/uni-fab.vue';
	import basic from '@/api/basic';
	import citySelect from '@/components/city-select/city-select.vue';
	import workshop from '@/api/workshop';
	import service from '@/service.js';
	import loading from '@/components/loading';
	import {
		mapState
	} from 'vuex';
	var tsc = require("@/util/ble/tsc.js");
	var esc = require("@/util/ble/esc.js");
	var encode = require("@/util/ble/encoding.js");
	export default {
		components: {
			ruiDatePicker,
			ldSelect,
			uniFab,
			loading,
			citySelect
		},
		data() {
			return {
				pageHeight: 0,
				headName: '',
				isOrder: false,
				isDis: false,
				onoff: true,
				isClick: false,
				loadModal: false,
				pickerVal: null,
				modalName: null,
				modalName2: null,
				gridCol: 3,
				formatName: 'FName',
				form: {

				},
				popupForm: {},
				skin: false,
				listTouchStart: 0,
				listTouchDirection: null,
				deptList: [],
				empList: [],
				shiftList: [],
				processList: [],
				horizontal: 'right',
				vertical: 'bottom',
				popMenu: false,
				direction: 'horizontal',
				pattern: {
					color: '#7A7E83',
					backgroundColor: '#fff',
					selectedColor: '#007AFF',
					buttonColor: '#007AFF'
				},
				cuIList: [],
				startDate: null,
				endDate: null,
				sendContent: "",
				looptime: 0,
				currentTime: 1,
				lastData: 0,
				oneTimeData: 0,
				returnResult: "",
				canvasWidth: 180,
				canvasHeight: 180,
				imageSrc: '../../static/img/abc_ic_star_black_16dp.png',
				buffSize: [],
				buffIndex: 0,
				printNum: [],
				printNumIndex: 0,
				printerNum: 1,
				currentPrint: 1,
				isReceiptSend: false,
				isLabelSend: false
			};
		},
		computed: mapState(['sysinfo', 'Bluetooth']),
		onLoad: function(option) {
			let me = this
			if (JSON.stringify(option) != "{}") {
				this.isOrder = true
				var obj = JSON.parse(decodeURIComponent(option.obj))
				me.form = obj;
				me.startDate = obj.startDate
				me.endDate = obj.endDate
				me.form.fprocessnote = me.form.FRemark
				me.form.FWBNuSentNum = me.form.FProcedurePlanNumber-me.form.FWBSentNum
			}
		},
		onUnload() {
			let that = this;
			let {
				BLEInformation
			} = that.Bluetooth;
			uni.closeBLEConnection({
				deviceId: BLEInformation.deviceId,
				success: function(res) {
					console.log("关闭蓝牙成功")
				},
			})
		},
		onReady: function() {
			var me = this
			me.loadModal = true
			if (service.getUsers().length > 0) {
				if (service.getUsers()[0].account != '' && service.getUsers()[0].account != "undefined") {
					me.form.fbillerID = service.getUsers()[0].userId
					me.form.username = service.getUsers()[0].username
					uni.getSystemInfo({
						success: function(res) { // res - 各种参数
							let info = uni.createSelectorQuery().select(".getheight");
							let customHead = uni.createSelectorQuery().select(".customHead");
							var infoHeight = 0;
							var headHeight = 0;
							info.boundingClientRect(function(data) { //data - 各种参数
								infoHeight = data.height
							}).exec();
							customHead.boundingClientRect(function(data) { //data - 各种参数
								headHeight = data.height
							}).exec();
							setTimeout(function() {
								me.pageHeight = res.windowHeight - infoHeight - headHeight -30
							}, 1000);
						}
					});
					me.initMain()


				}
			}
			let list = []
			let numList = []
			let j = 0
			for (let i = 20; i < 200; i += 10) {
				list[j] = i;
				j++
			}
			for (let i = 1; i < 10; i++) {
				numList[i - 1] = i
			}
			this.buffSize = list;
			this.oneTimeData = list[0];
			this.printNum = numList;
			this.printerNum = numList[0];

		},
		onShow() {
			let that = this;
			let width;
			let height;
			uni.getImageInfo({
				src: that.imageSrc,
				success(res) {
					console.log(res.width)
					console.log(res.height)
					width = res.width
					height = res.height
					that.canvasWidth = res.width;
					that.canvasHeight = res.height;
				}
			})
			const ctx = uni.createCanvasContext("edit_area_canvas", this);
			// if (app.globalData.platform == "android") {
			//   ctx.translate(width, height)
			//   ctx.rotate(180 * Math.PI / 180)
			// }
			ctx.drawImage(this.imageSrc, 0, 0, width, height);
			ctx.draw();
		},
		methods: {
			sumCount(val, item) {
				this.$set(item, 'FSendQty', val.detail.value);
			},
			sumRemark(val) {
				this.form.fprocessnote = val.detail.value;
			},
			cityClick(item) {
				this.$set(this.popupForm, 'userName', item.FName);
				this.$set(this.popupForm, 'userId', item.FNumber);
				this.modalName = null
			},
			clearList() {
				const that = this
				if (that.cuIList.length > 0) {
					uni.showModal({
						title: '温馨提示',
						content: '是否清空列表，清空之后将无法还原！',
						success: function(res) {
							if (res.confirm) {
								that.cuIList = []
								that.initMain()
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				}
			},
			initMain() {
				const me = this
				this.form.workDate = this.getDay('', 0).date
				basic.getDeptList({}).then(res => {
					if (res.success) {
						me.deptList = res.data
					}
				}).catch(err => {
					uni.showToast({
						icon: 'none',
						title: err.msg,
					});
				});
				basic.getShiftList({}).then(res => {
					if (res.success) {
						res.data.forEach((item) => {
							item.FNumber = item.FNumber.toString();
						})
						me.shiftList = res.data
						me.cuIList.push({
							userId: '',
							userName: '',
							FSendQty: me.form.FWBNuSentNum,
							FClassNumber: me.shiftList[0].FNumber,
							FClassName: me.shiftList[0].FName,
							FDeptID: '',
						})
						
					}
				}).catch(err => {
					uni.showToast({
						icon: 'none',
						title: err.msg,
					});
				});
				basic.getEmpList({}).then(res => {
					if (res.success) {
						me.empList = res.data
						
					}
				}).catch(err => {
					uni.showToast({
						icon: 'none',
						title: err.msg,
					});
				});
				me.loadModal = false
				me.isClick = false
			},
			saveData() {
				this.isClick = true
				let result = []
				let portData = {};
				let list = this.cuIList
				let me = this
				let array = []
				basic
					.getBillNo({
						TranType: 1002534
					})
					.then(reso => {
						if (reso.success) {
							for (let i in list) {
								let obj = {}
								if (list[i].userId == null || typeof list[i].userId == '') {
									result.push(list[i].index);
								}
								if (list[i].FClassNumber == null || typeof list[i].FClassNumber == '') {
									result.push(list[i].index);
								}
								if (list[i].FSendQty == null || typeof list[i].FSendQty == '') {
									result.push(list[i].index);
								}
								obj.fopernumber = list[i].userId
								obj.fclassnumber = list[i].FClassNumber
								obj.fsendqty = list[i].FSendQty
								obj.fbillno = reso.data
								obj.fentryid = 0
								obj.workdate = me.form.workDate
								obj.fprocessnote = me.form.fprocessnote
								obj.productworkdetailid = me.form.productWorkDetailId
								array.push(obj)
							}
							if (result.length > 0) {
								me.isClick = false
								return uni.showToast({
									icon: 'none',
									title: '派工数，操作人，班次输入有误，请检查',
								});
							}
							if (array.length <= 0) {
								me.isClick = false
								return uni.showToast({
									icon: 'none',
									title: '无派工数据',
								});
							}
							portData.ftrantype = "1002534"
							portData.foper = "N"
							portData.finterid = 0
							portData.fdate = me.form.FDate
							portData.fbillno = reso.data
							portData.fplanbillno = me.form.FBillNo
							portData.ficmobillno = me.form.FProduceTaskNo
							portData.fplanentryid = me.form.FEntryID
							portData.fplanstartdate = me.form.FPlanStartDate
							portData.fplanenddate = me.form.FPlanEndDate
							portData.fitemnumber = me.form.FItemNumber
							portData.forderno = me.form.FOrderNo
							portData.fwbnumber = me.form.FAlternateNumber
							portData.finterid = 0
							portData.repEntry = array
							workshop.productWorkInsert(portData).then(res => {
								if (res.success) {
									uni.showToast({
										icon: 'success',
										title: res.msg,
									});
									me.form.bNum = 0
									if (me.isOrder) {
										uni.showModal({
											title: "提示",
											content: "是否立即打印",
											showCancel: true,
											cancelText: '取消',
											confirmText: '确定',
											success: res => {
												console.log(res.confirm)
												if (res.confirm) {
													let {
														BLEInformation
													} = me.Bluetooth;
													me.form.fdate = me.form.FDate
													me.form.fbillno = reso.data
													me.form.entry = me.cuIList
													if (BLEInformation.deviceId == "") {
														// 用户点击确定
														setTimeout(function() {
															uni.redirectTo({
																url: '/pages/bleConnect/bleConnect?obj=' +
																	encodeURIComponent(
																		JSON
																		.stringify(
																			me.form
																			))
															});
														}, 1000)
													} else {
														me.bindViewTap();
													}
												} else {
													setTimeout(function() {
														uni.$emit('handleBack', {
															startDate: me.startDate,
															endDate: me.endDate
														});
														uni.navigateBack({
															url: '../workshop/dispatching'
														});
													}, 1000)
													// 否则点击了取消  
												}
											}
										})
									}
								}
							}).catch(err => {
								uni.showToast({
									icon: 'none',
									title: err.message,
								});
								this.isClick = false
							})

						}
					})
					.catch(err => {
						uni.showToast({
							icon: 'none',
							title: err.msg
						});
					});
			},
			showModal(index, item) {
				this.modalName = 'Modal'
				this.popupForm = item
			},
			hideModal(e) {
				this.modalName = null
			},
			del(index, item) {
				this.cuIList.splice(index, 1)
				var count = 0
				for (var i = 0; i < list.length; i++) {
					count += Number(list[i].dispatchNum)
				}
				this.form.bNum = count
			},
			// 查询前后三天日期
			getDay(date, day) {
				var today = new Date();
				var targetday_milliseconds = today.getTime() + 1000 * 60 * 60 * 24 * day
				today.setTime(targetday_milliseconds) //注意，这行是关键代码
				var tYear = today.getFullYear()
				var tMonth = today.getMonth()
				var tDate = today.getDate()
				var getDay = today.getDay()
				tMonth = this.doHandleMonth(tMonth + 1)
				tDate = this.doHandleMonth(tDate)
				var weeks = new Array("星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六");
				var week = weeks[getDay]
				return {
					day: tDate,
					week: week,
					date: tYear + "-" + tMonth + "-" + tDate
				}
			},
			doHandleMonth(month) {
				var m = month;
				if (month.toString().length == 1) {
					m = "0" + month;
				}
				return m;
			},
			deptChange(val) {
				this.form.fdeptID = val
			},
			shiftChange(val, item) {
				item.FClassNumber = val
			},
			processChange(val) {
				this.form.processID = val
			},
			PickerChange(e, item) {
				this.$set(item,'FClassName', this.shiftList[e.detail.value].FName);
				this.$set(item,'FClassNumber', this.shiftList[e.detail.value].FNumber);
			},
			/* PickerChange(e, item) {
				this.$set(item, 'userName', this.empList[e.detail.value].FName);
				this.$set(item, 'userId', this.empList[e.detail.value].FNumber);
			}, */
			bindChange(e) {
				this.form.fdate = e
			},
			fabClick() {
				var that = this
				that.cuIList.push({
					userId: '',
					userName: '',
					FSendQty: 0,
					FClassNumber: that.shiftList[0].FNumber,
					FClassName: that.shiftList[0].FName,
					FDeptID: '',
				})
				that.$forceUpdate();
			}, // ListTouch触摸开始
			ListTouchStart(e) {
				this.listTouchStart = e.touches[0].pageX
			},

			// ListTouch计算方向
			ListTouchMove(e) {
				this.listTouchDirection = e.touches[0].pageX - this.listTouchStart > 0 ? 'right' : 'left'
			},

			// ListTouch计算滚动
			ListTouchEnd(e) {
				if (this.listTouchDirection == 'left') {
					this.modalName = e.currentTarget.dataset.target
				} else {
					this.modalName = null
				}
				this.listTouchDirection = null
			},
			//绑定蓝牙
			bindViewTap(e) {
				let that = this;
				let {
					BLEInformation
				} = that.Bluetooth;
				let
					title = BLEInformation.deviceId;
				that.serviceId = 0;
				that.writeCharacter = false;
				that.readCharacter = false;
				that.notifyCharacter = false;
				uni.showLoading({
					title: '正在连接',
				})
				console.log('deviceId:', title)
				// uni.createBLEConnection
				plus.bluetooth.createBLEConnection({
					deviceId: title,
					success(res) {
						console.log('createBLEConnection success:', res)
						BLEInformation.deviceId = title;
						uni.hideLoading()
						that.getSeviceId()
					},
					fail(e) {
						console.log('createBLEConnection error:', e)
						that.errorCodeTip(e.errCode);
						uni.hideLoading()
					}
				})
			},
			//获取蓝牙设备所有服务(service)。
			getSeviceId() {
				let that = this;
				let {
					BLEInformation
				} = that.Bluetooth;
				console.log('BLEInformation.deviceId:', BLEInformation.deviceId)
				// uni.getBLEDeviceServices
				let t = setTimeout(() => {
					plus.bluetooth.getBLEDeviceServices({
						deviceId: BLEInformation.deviceId,
						success(res) {
							console.log('getBLEDeviceServices success:', res)
							that.services = res.services;
							that.getCharacteristics()
						},
						fail: function(e) {
							that.errorCodeTip(e.code);
							console.log('getBLEDeviceServices fail:', e)
						}
					})
					clearTimeout(t);
				}, 1500)
			},
			getCharacteristics() {
				var that = this
				let {
					services: list,
					serviceId: num,
					writeCharacter: write,
					readCharacter: read,
					notifyCharacter: notify
				} = that;
				let {
					BLEInformation
				} = that.Bluetooth;
				// uni.getBLEDeviceCharacteristics
				plus.bluetooth.getBLEDeviceCharacteristics({
					deviceId: BLEInformation.deviceId,
					serviceId: list[num].uuid,
					success(res) {
						console.log(res)
						for (var i = 0; i < res.characteristics.length; ++i) {
							var properties = res.characteristics[i].properties
							var item = res.characteristics[i].uuid
							if (!notify) {
								if (properties.notify) {
									BLEInformation.notifyCharaterId = item;
									BLEInformation.notifyServiceId = list[num].uuid;
									that.$store.commit('BLEInformationSet', BLEInformation);
									notify = true
								}
							}
							if (!write) {
								if (properties.write) {
									BLEInformation.writeCharaterId = item;
									BLEInformation.writeServiceId = list[num].uuid;
									that.$store.commit('BLEInformationSet', BLEInformation);
									write = true
								}
							}
							if (!read) {
								if (properties.read) {
									BLEInformation.readCharaterId = item;
									BLEInformation.readServiceId = list[num].uuid;
									that.$store.commit('BLEInformationSet', BLEInformation);
									read = true
								}
							}
						}
						if (!write || !notify || !read) {
							num++
							that.writeCharacter = write;
							that.readCharacter = read;
							that.notifyCharacter = notify;
							that.serviceId = num;
							if (num == list.length) {
								uni.showModal({
									title: '提示',
									content: '找不到该读写的特征值',
								})
							} else {
								that.getCharacteristics()
							}
						} else {
							that.openControl()
						}
					},
					fail: function(e) {
						console.log("getBLEDeviceCharacteristics fail：", e);
						that.errorCodeTip(e.errCode);
					}
				})
			},
			openControl: function() {
				let that = this;
				for (let i = 0; i < that.form.entry.length; i++) {
					if (i == 0) {
						that.labelTest(that.form.entry[i]);
					} else {
						(function(t, data) { // 注意这里是形参
							setTimeout(function() {
								that.labelTest(that.form.entry[i]);
							}, 3000 * t); // 还是每秒执行一次，不是累加的
						})(i, '') // 
					}
				}
				setTimeout(function() {
					uni.$emit('handleBack', {
						startDate: me.startDate,
						endDate: me.endDate
					});
					uni.navigateBack({
						url: '../workshop/dispatching'
					});
				}, 1000)
				/* uni.navigateTo({
					url: '/pages/sendCommand/sendCommand',
				}) */
			},
			//标签打印
			labelTest(item) {
				console.log("打印")
				console.log(item)
				let that = this;
				let canvasWidth = that.canvasWidth
				let canvasHeight = that.canvasHeight
				let command = tsc.jpPrinter.createNew()
				command.setSize(80, 60)
				command.setGap(0)
				command.setCls()
				command.setQR(420, 300, "L", 6, "A", that.form.fbillno)
				/* command.setBarCode(180, 350, "39", 96, 1, 2, 2, that.form.fbillno) */
				command.setText(1, 20, "TSS24.BF2", 1, 1, '生产任务单:' + that.form.FProduceTaskNo)
				command.setText(1, 60, "TSS24.BF2", 1, 1, '物料编码:' + that.form.FItemNumber)
				let num = 100;
				that.form.FItemName = '物料名称:' + that.form.FItemName
				for (var i = 0; i < Math.ceil(that.form.FItemName.length / 31); i++) {
					num = num + (i * 40)
					command.setText(1, num, "TSS24.BF2", 1, 1, that.form.FItemName.slice(i * 31, i * 31 + 31));
				};
				num = num + 40
				that.form.fprocessnote = '加工说明:' + that.form.fprocessnote
				for (var i = 0; i < Math.ceil(that.form.fprocessnote.length / 31); i++) {
					num = num + (i * 40)
					command.setText(1, num, "TSS24.BF2", 1, 1, that.form.fprocessnote.slice(i * 31, i * 31 + 31));
				};
				command.setText(1, num + 40, "TSS24.BF2", 1, 1, '员工:' + item.userName)
				command.setText(1, num + 80, "TSS24.BF2", 1, 1, '数量:' + item.FSendQty)
				command.setText(1, num + 120, "TSS24.BF2", 1, 1, '工序名称:' + that.form.FAlternateName)
				command.setPagePrint();
				that.isLabelSend = true;
				that.prepareSend(command.getData())
			},
			//准备发送，根据每次发送字节数来处理分包数量
			prepareSend(buff) {
				console.log(buff);
				let that = this
				let time = that.oneTimeData
				let looptime = parseInt(buff.length / time);
				let lastData = parseInt(buff.length % time);
				console.log(looptime + "---" + lastData)
				this.looptime = looptime + 1;
				this.lastData = lastData;
				this.currentTime = 1;
				that.Send(buff)
			},
			//分包发送
			Send(buff) {
				let that = this
				let {
					currentTime,
					looptime: loopTime,
					lastData,
					oneTimeData: onTimeData,
					printerNum: printNum,
					currentPrint
				} = that;
				let buf;
				let dataView;
				if (currentTime < loopTime) {
					buf = new ArrayBuffer(onTimeData)
					dataView = new DataView(buf)
					for (var i = 0; i < onTimeData; ++i) {
						dataView.setUint8(i, buff[(currentTime - 1) * onTimeData + i])
					}
				} else {
					buf = new ArrayBuffer(lastData)
					dataView = new DataView(buf)
					for (var i = 0; i < lastData; ++i) {
						dataView.setUint8(i, buff[(currentTime - 1) * onTimeData + i])
					}
				}
				console.log("第" + currentTime + "次发送数据大小为：" + buf.byteLength)
				let {
					BLEInformation
				} = that.Bluetooth;

				plus.bluetooth.writeBLECharacteristicValue({
					deviceId: BLEInformation.deviceId,
					serviceId: BLEInformation.writeServiceId,
					characteristicId: BLEInformation.writeCharaterId,
					value: buf,
					success: function(res) {
						console.log(res)
					},
					fail: function(e) {
						console.log(e)
					},
					complete: function() {
						currentTime++
						if (currentTime <= loopTime) {
							that.currentTime = currentTime;
							that.Send(buff)
						} else {
							uni.showToast({
								title: '已打印第' + currentPrint + '张',
							})
							if (currentPrint == printNum) {
								that.looptime = 0;
								that.lastData = 0;
								that.currentTime = 1;
								that.isReceiptSend = false;
								that.isLabelSend = false;
								that.currentPrint = 1;
							} else {
								currentPrint++;
								that.currentPrint = currentPrint;
								that.currentTime = 1;
								that.Send(buff)
							}
						}
					}
				})
			},
		}
	}
</script>

<style>
	.cu-item {
		float: left;
		width: 50%;
	}

	.cu-item .content {
		float: left;
	}

	.cu-list.menu-avatar>.cu-item .content {
		left: 5px;
	}

	.cu-list.menu-avatar>.cu-item .action {}

	.input {
		height: 30px;
	}

	.box {
		width: 100%;
	}

	.uni-input-placeholder,
	.uni-input-input {
		font-size: 13px;
	}

	.action,
	.content {
		font-size: 13px !important;
	}

	.ruidata {
		font-size: 13px;
		height: 7vw !important;
	}

	.cu-bar {
		min-height: 30px;
	}

	/* .page {
		height: calc(100vh - 320upx);
	} */
	.nav-title::first-letter {
		font-size: 16px;
		margin-right: 2px;
	}
</style>
