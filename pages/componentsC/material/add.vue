<template>
	<view class="bianjia u-demo-block__content">
		<u-notice-bar text="居民信息以房屋认证信息为主,如有变动请联系工作人员!"></u-notice-bar>
		<u--form labelPosition="left" :model="model" ref="form1">
			
			<u-form-item label="选择区域:" borderBottom ref="item1" labelWidth="100" 
				@click="showSex = true;">
				<u--input v-model="model.type" disabled disabledColor="#ffffff" placeholder="请选择区域" border="none">
				</u--input>
				<u-icon slot="right" name="arrow-right"></u-icon>
			</u-form-item>
			<u-form-item label="物资清单:" borderBottom ref="item1" labelWidth="100" >
				<u--textarea placeholder="请具体描述物资清单信息" v-model="model.remarks" count maxlength="200"></u--textarea>
			</u-form-item>
		</u--form>
		<view class="but" @click="tijiao()">提交</view>

		<u-action-sheet :show="showSex" :actions="radiolist2" title="选择区域" @close="showSex = false"
			@select="sexSelect">
		</u-action-sheet>
	</view>
</template>

<script>
	import action from '@/common/util.js'
	export default {
		data() {
			return {
				showSex: false,
				lx: 0,
				model: {
					wxid:uni.getStorageSync('userData').uid,
					houseid:0,
					remarks: '',
					type: '',
				},

				radiolist2: [],
				fileList4: [],
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
			this.gethome()
		},
		methods: {
			gethome() {
				uni.$u.http.post('House/bydate', {id:uni.getStorageSync('userData').uid}).then(res => {
					let arr=[];
					for(let i=0;i<res.data.length;i++){
						arr.push({
							id:res.data[i].id,
							name: res.data[i].title+"-"+res.data[i].unit+"单元-"+res.data[i].name,
						})
					}
					
					this.radiolist2 = arr;
					
				})
			},
			sexSelect(e) {
				console.log(e)
				this.model.houseid = e.id
				this.model.type = e.name
			},
			
			tijiao(){
				uni.$u.http.post('Material/edit', this.model)
				.then(res => {
				
					if(res.code==200){
						uni.showToast({
							title: res.msg,
							icon: 'success',
							duration: 2000
						})
					}else{
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
	page{
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
</style>
