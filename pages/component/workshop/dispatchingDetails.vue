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
				<view class="action">
					<text>生产数量:{{form.FWBSentNum}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom flex-wrap" style="height: auto;">
				<view class="action">
					<text>产品编码:{{form.FItemNumber}}</text>
				</view>
				
			</view><view class="cu-bar bg-white solid-bottom flex-wrap" style="height: auto;">
				
				<view class="action">
					<text>产品名称:{{form.FItemName}}</text>
				</view>
			</view>
			<view class="cu-bar bg-white solid-bottom" style="height: auto;">
				<view class="action">
					<text>规格型号:{{form.FModel}}</text>
				</view>
				<view class="action">
					<text>工序序号:{{form.FOrderNo}}</text>
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
					<view class="cu-item" style="width: 100%;margin-top: 2px;padding:15rpx 0 15rpx 0;height: auto;"
						:class="modalName=='move-box-'+ index?'move-cur':''" @touchstart="ListTouchStart"
						@touchmove="ListTouchMove" @touchend="ListTouchEnd" :data-target="'move-box-' + index">
						<view style="clear: both;width: 100%;" class="grid text-center col-2">
								<view>
									<ld-select :list="shiftList" list-key="FName" value-key="FNumber" placeholder="班次" clearable
										v-model="item.FClassNumber" @change="shiftChange($event, item)"></ld-select>
								</view>
							<view>
								<view style="float: left;line-height: 60upx;">派工量:</view>
								<input name="input" type="number" style="border-bottom: 1px solid;"
									@input='sumCount($event, item)' v-model="item.FSendQty" />
							</view>
							<view class="padding-top">
								<button class="cu-btn sm round bg-green shadow" @tap="showModal(index, item)"
									:disabled="isDis" data-target="Modal">
									<text class="cuIcon-people">
									</text>操作人:{{item.userName}}</button>
							</view>
						</view>
						<view class="move">
							<view class="bg-red" @tap="del(index,item)">删除</view>
						</view>
					</view>
				</view>
			</view>
			<view class="cu-bar tabbar shadow foot">
				<view class="box text-center">
					<button :disabled="isClick" class="cu-btn bg-green shadow-blur round lg"
						style="width: 40%;" @tap="$manyCk(saveData)">提交</button>
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
			};
		},
		onLoad: function(option) {
			let me = this
			if (JSON.stringify(option) != "{}") {
				this.isOrder = true
				var obj = JSON.parse(decodeURIComponent(option.obj))
				me.form = obj;
				me.startDate = obj.startDate
				me.endDate = obj.endDate
				/* me.form.kingDeeNo = option.kingDeeNo
				me.form.productWorkDetailId = option.productWorkDetailId
				me.form.lotNo = option.lotNo
				me.form.model = option.model
				me.form.planNum = option.planNum
				me.form.processCard = option.processCard
				me.form.productName = option.productName
				me.form.productNumber = option.productNumber
				me.form.workNo = option.workNo
				me.startDate = option.startDate
				me.endDate = option.endDate
				workshop.formatByProductWork({
					productWorkDetailId: option.productWorkDetailId
				}).then(res => {
					if (res.success) {
						me.processList = res.data
					}
				}).catch(err => {
					uni.showToast({
						icon: 'none',
						title: err.msg,
					});
				})*/
			} 
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
								me.pageHeight = res.windowHeight - infoHeight - headHeight
							}, 1000);
						}
					});
					me.initMain()


				}
			}

		},
		methods: {
			sumCount(val, item) {
				var list = this.cuIList
				this.$set(item, 'dispatchNum', val.detail.value);
				var count = 0
				for (var i = 0; i < list.length; i++) {
					count += Number(list[i].dispatchNum)
				}
				console.log(count)
				this.form.bNum = count
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
						res.data.forEach((item)=>{
							item.FNumber = item.FNumber.toString();
						})
						me.shiftList = res.data
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
					.getBillNo({ TranType: 1002534 })
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
								obj.workdate = this.form.workDate
								obj.productworkdetailid = this.form.productWorkDetailId
								array.push(obj)
							}
							if (result.length > 0) {
								this.isClick = false
								return uni.showToast({
									icon: 'none',
									title: '派工数，操作人，班次输入有误，请检查',
								});
							}
							if (array.length <= 0) {
								this.isClick = false
								return uni.showToast({
									icon: 'none',
									title: '无派工数据',
								});
							}
							portData.ftrantype = "1002534"
							portData.foper = "N"
							portData.finterid = 0
							portData.fdate = this.form.FDate
							portData.fbillno = reso.data
							portData.fplanbillno = this.form.FBillNo
							portData.ficmobillno = this.form.FProduceTaskNo
							portData.fplanentryid = this.form.FEntryID
							portData.fplanstartdate = this.form.FPlanStartDate
							portData.fplanenddate = this.form.FPlanEndDate
							portData.fitemnumber = this.form.FItemNumber
							portData.forderno = this.form.FOrderNo
							portData.fwbnumber = this.form.FAlternateNumber
							portData.finterid = 0
							portData.repEntry = array
							workshop.productWorkInsert(portData).then(res => {
								if (res.success) {
									this.cuIList = []
									uni.showToast({
										icon: 'success',
										title: res.msg,
									});
									this.form.bNum = 0
									this.initMain()
									if (this.isOrder) {
										setTimeout(function() {
											uni.$emit("handleBack", {
												startDate: me.startDate,
												endDate: me.endDate
											});
											uni.navigateBack({
												url: '../workshop/dispatching'
											});
										}, 1000)
									}
								}
							}).catch(err => {
								uni.showToast({
									icon: 'none',
									title: err.msg,
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
			},shiftChange(val,item) {
				item.FClassNumber = val
			},
			processChange(val) {
				this.form.processID = val
			},
			PickerChange(e, item) {
				this.$set(item, 'userName', this.empList[e.detail.value].FName);
				this.$set(item, 'userId', this.empList[e.detail.value].FNumber);
			},
			bindChange(e) {
				this.form.fdate = e
			},
			fabClick() {
				var that = this
				that.cuIList.push({
					userId: '',
					dispatchNum: 0,
					FClassNumber: '',
				})
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
			}
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
