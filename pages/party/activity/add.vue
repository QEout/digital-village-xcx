<template>
	<view class="bianjia u-demo-block__content">

		<u--form labelPosition="left" :model="model" ref="form1">
    			<u-form-item label="选择身份信息:" borderBottom ref="item1" labelWidth="100" 
				@click="showWork = true;">
				<u--input v-model="model.type" disabled disabledColor="#ffffff"  border="none">
				</u--input>
				<u-icon slot="right" name="arrow-right"></u-icon>
			</u-form-item>
			<u-form-item label="备用联系方式:" borderBottom ref="item1" labelWidth="100">
				<u--input v-model="model.bphone" border="none" placeholder="请输入备用联系方式" maxlength="10" inputAlign="right">
				</u--input>
			</u-form-item>
			<u-form-item label="备注:" borderBottom ref="item1" labelWidth="100" >
				<u--textarea placeholder="备注" v-model="model.benks" count maxlength="200"></u--textarea>
			</u-form-item>
		</u--form>
		<view class="but mt-8" @click="tijiao()">提交</view>

		<u-action-sheet :show="showWork" :actions="radiolist2" title="选择身份信息" @close="showWork = false"
			@select="workSelect">
		</u-action-sheet>
	</view>
</template>

<script>
	import action from '@/common/util.js'
	export default {
		data() {
			return {
				showWork: false,
				lx: 0,
				model: {
					wxid:uni.getStorageSync('userData').uid,
					workid:0,
					id:0,
					benks: '',
					bphone: '',
					type:''
				},
				radiolist2:[]
				
			}
		},
		onLoad(e) {
			this.model.id=e.id
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
					this.radiolist2 = uni.getStorageSync('userData').workinfo.filter(item => item.cname=='CPC').map(item => {
            return {
              id: item.id,
              name: item.name+' - '+item.zw
            }
          })

			},
			workSelect(e) {
				console.log(e)
				this.model.workid = e.id
				this.model.type = e.name
			},
			
			tijiao(){
				uni.$u.http.post('ActivityUser/add', this.model)
				.then(res => {
					// this.list=res.data;
					console.log(res)
					// setTimeout(() => {
						
					// }, 1000)
				
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
