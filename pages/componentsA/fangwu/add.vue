<template>
	<view class="bianjia u-demo-block__content">
		<u--form labelPosition="left" :model="model" ref="form1">
			<u-form-item label="选择区域:" borderBottom ref="item1" labelWidth="100" @click="toggleMaskLocation">
				<u--input v-model="fangwu" border="none" placeholder="请选择区域" :disabled="true" disabledColor="#fff"
					inputAlign="right"></u--input>
				<u-icon slot="right" name="arrow-right"></u-icon>
			</u-form-item>
			<u-form-item label="住户类型:" borderBottom ref="item1" labelWidth="100"
				@click="showSex = true;">
				<u--input v-model="model.relationship" disabled disabledColor="#ffffff" placeholder="请选择类型"
					border="none" inputAlign="right">
				</u--input>
				<u-icon slot="right" name="arrow-right"></u-icon>
			</u-form-item>

			<u-form-item label="姓名:" borderBottom ref="item1" labelWidth="100">
				<u--input v-model="model.name" border="none" placeholder="请输入姓名" maxlength="10" inputAlign="right">
				</u--input>
			</u-form-item>
			<u-form-item label="性别:" borderBottom ref="item1" labelWidth="100" >
				<u-radio-group v-model="model.sex">
					<u-radio :customStyle="{marginRight: '16px'}" v-for="(item, index) in radiolist1" :key="index"
						:label="item.name" :name="item.id">
					</u-radio>
				</u-radio-group>
			</u-form-item>
			<u-form-item label="联系电话:" borderBottom ref="item1" labelWidth="100" v-if="shid==0">
				<u--input v-model="model.phone" border="none" placeholder="请输入联系电话" inputAlign="right"></u--input>
			</u-form-item>
			<u-form-item label="身份证号:" borderBottom ref="item1" labelWidth="100" v-if="shid==0">
				<u--input v-model="model.idcard" border="none" placeholder="请输入身份证号" inputAlign="right"></u--input>
			</u-form-item>
			<u-form-item label="身份证正面:" borderBottom ref="item1" labelWidth="100" v-if="shid==0">
				<u-upload :fileList="fileList1" @afterRead="afterRead" @delete="deletePic" name="1" multiple
					:maxCount="1"></u-upload>
			</u-form-item>
			<u-form-item label="身份证反面:" borderBottom ref="item1" labelWidth="100" v-if="shid==0">
				<u-upload :fileList="fileList2" @afterRead="afterRead" @delete="deletePic" name="2" multiple
					:maxCount="1"></u-upload>
			</u-form-item>
			<u-form-item label="房产证照片:" borderBottom ref="item1" labelWidth="100" v-if="model.relationship!='租户' && shid==0" >
				<u-upload :fileList="fileList3" @afterRead="afterRead" @delete="deletePic" name="3" multiple
					:maxCount="1"></u-upload>
			</u-form-item>
			<u-form-item label="租房合同:" borderBottom ref="item1" labelWidth="100" v-if="model.relationship=='租户'  && shid==0"> 
				<u-upload :fileList="fileList3" @afterRead="afterRead" @delete="deletePic" name="3" multiple
					:maxCount="1"></u-upload>
			</u-form-item>
			


		</u--form>
		<view class="but" @click="tijiao()">提交</view>
		<gk-city :headtitle="headtitle" :provincedata="provincedata" :data="selfData" mode="cityPicker" ref="cityPicker"
			@funcvalue="getpickerParentValue" :pickerSize="3"></gk-city>

		<u-action-sheet :show="showSex" :actions="radiolist2" title="请选择类型" @close="showSex = false"
			@select="sexSelect">
		</u-action-sheet>
	</view>
</template>

<script>
	import action from '@/common/util.js'

	import gkcity from '@/components/gk-city/gk-city.vue';
	export default {
		components: {
			gkcity
		},
		data() {
			return {
				shid: 1,
				provincedata: [],
				selfData: [],
				showdata: false,
				list: [],
				indexs: [0, 0, 0],
				showSex: false,
				lx: 0,
				show3: false,
				columns3: [],
				radiolist2: [{
						name: '本人',
						disabled: false
					},
					{
						name: '家人',
						disabled: false
					},
					{
						name: '租户',
						disabled: false
					}
					// ,
					// {
					// 	name: '亲戚',
					// 	disabled: false
					// }
				],
				radiolist1: [{
						id:1,
						name: '男',
						disabled: false
					},
					{
						id:2,
						name: '女',
						disabled: false
					}
				],
				model: {
					wxid: uni.getStorageSync('userData').uid,
					name: '',
					houseid: 0,
					sex: 1,
					phone: '',
					idcard: '',
					relationship: '',
					ioc1: '',
					ioc2: '',
					ioc3: ''
				},
				fangwu: "",
				fileList1: [],
				fileList2: [],
				fileList3: []
			}
		},
		onLoad(e) {
			
			uni.hideShareMenu();
			uni.onCopyUrl((result) => {
				setTimeout(() => {
				  uni.setClipboardData({
					data: "不可复制哦",
				  });
				}, 1000);
			  });
		},
		onShow() {
			this.shid=uni.getStorageSync('userData').shid
			uni.$u.http.post('OwnerMember/fangzi', {}).then(res => {
				this.list = res.data;
			})
		},
		methods: {
			toggleMaskLocation() {
				this.selfData = this.list;
				this.$nextTick(() => {
					this.$refs["cityPicker"].show();
				})
			},
			getpickerParentValue(data) {
				this.fangwu = data[0].text + "-" + data[1].text + "-" + data[2].text;
				this.model.houseid = data[2].value
				// console.log("data",data);  //获取地址的value值
				// console.log(data.map(o=>{return o.value}));  //获取地址的value值
				// this.provincedata=data;
				// this.addressByPcrs=data.map(o=>{return o.text}).join(" ")
			},
			sexSelect(e) {
				this.model.relationship = e.name
			},
			// 删除图片
			deletePic(event) {
				this[`fileList${event.name}`].splice(event.index, 1)
			},
			// 新增图片
			async afterRead(event) {
				// 当设置 mutiple 为 true 时, file 为数组格式，否则为对象格式
				let lists = [].concat(event.file)
				let fileListLen = this[`fileList${event.name}`].length
				lists.map((item) => {
					this[`fileList${event.name}`].push({
						...item,
						status: 'uploading',
						message: '上传中'
					})
				})
				for (let i = 0; i < lists.length; i++) {
					const result = await this.uploadFilePromise(lists[i].url)
					// console.log(result)
					let item = this[`fileList${event.name}`][fileListLen]
					this[`fileList${event.name}`].splice(fileListLen, 1, Object.assign(item, {
						status: 'success',
						message: '',
						url: result
					}))
					fileListLen++
				}
			},
			uploadFilePromise(url) {
				let ioc = action.imgurl
				return new Promise((resolve, reject) => {
					let a = uni.uploadFile({
						url: ioc, // 仅为示例，非真实的接口地址
						filePath: url,
						name: 'file',
						formData: {
							user: 'test'
						},
						success: (res) => {
							// console.log(JSON.parse(res.data).data)
							setTimeout(() => {
								resolve(JSON.parse(res.data).data)
							}, 1000)
						}
					});
				})
			},
			tijiao() {
				// console.log(this.fileList4)
				if(this.fileList1.length>0){
					this.model.ioc1=this.fileList1[0].url;
				}
				if(this.fileList2.length>0){
					this.model.ioc2=this.fileList2[0].url;
				}
				if(this.fileList3.length>0){
					this.model.ioc3=this.fileList3[0].url;
				}
				
				// console.log(this.model)
				uni.$u.http.post('OwnerMember/renzheng', this.model)
					.then(res => {
						// this.list=res.data;
						console.log(res)

						if (res.code == 200) {
							uni.showToast({
								title: res.msg,
								icon: 'success',
								duration: 2000
							})
              // 返回到上一级
              setTimeout(() => {
                uni.navigateBack()
              }, 1000)
						} else {
							uni.showToast({
								title: res.msg,
								icon: 'none',
								duration: 2000
							})
						}
					})
			}
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #fff;
	}

	.bianjia {
		padding: 10rpx 40rpx;
	}

	.but {
		border-radius: 30rpx;
		background-color: #2b85e4;
		color: #fff;
		text-align: center;
		line-height: 80rpx;
		font-weight: 600;
		margin-bottom: 20rpx;
	}

	.yanghisb {
		display: inline;
	}

	.xanze {
		width: 100rpx;
		float: left;
		text-align: center;

	}

	.yt-list {
		background: #fff;
	}

	.yt-list-cell-warnning {
		display: flex;
		padding: 10rpx 30rpx 10rpx 40rpx;
		color: red;
	}

	.yt-list-cell {
		display: flex;
		align-items: center;
		padding: 6px 12px 6px 17px;
		line-height: 70rpx;
		position: relative;

		&.cell-hover {
			background: #fafafa;
		}

		&.b-b:after {
			left: 30rpx;
			right: 30rpx;

		}

		.gray {
			flex: 1;
			font-size: 28rpx;
			margin-right: 10rpx;
			height: 25px;
			line-height: 20px;
			color: gray;
		}

		.cell-icon {
			height: 32rpx;
			width: 32rpx;
			font-size: 28rpx;
			color: #fff;
			text-align: center;
			line-height: 32rpx;
			background: #f85e52;
			border-radius: 4rpx;
			margin-right: 12rpx;

			&.hb {
				background: #ffaa0e;
			}

			&.lpk {
				background: #3ab54a;
			}

		}

		.cell-more {
			align-self: center;
			font-size: 28rpx;
			margin-left: 8rpx;
			margin-right: -10rpx;
		}

		.cell-tit {
			flex: 1;
			font-size: 14px;
			margin-right: 10rpx;
		}

		.money {

			font-weight: bold;
			margin-left: 10px;
		}

		.picker-class {
			display: inline-block;
		}

		.cell-tip {
			font-size: 14px;
			flex-direction: column;

			&.disabled {}

			&.active {}

			&.red {}
		}

		&.desc-cell {
			.cell-tit {
				max-width: 90rpx;
			}
		}

		.desc {
			flex: 1;
			font-size: 14px;
		}
	}

	.b-b:after,
	.b-t:after {
		position: absolute;
		z-index: 3;
		bottom: 0;
		left: 0;
		right: 0;
		height: 0;
		content: '';
		-webkit-transform: scaleY(0.5);
		-ms-transform: scaleY(0.5);
		transform: scaleY(0.5);
		border-bottom: 1px solid #E4E7ED;
	}

	.red_color {
		color: red;
		display: inline-flex;
		padding-right: 6px;
	}
</style>
