<template>
	<view>
		<view class="body_gong" v-if="info">
			<view class="title_jz">{{info.title}} </view>
			<u-line margin="20rpx 0rpx"></u-line>
			<view class="">
				<u-parse :content="info.content"></u-parse>
			</view>
			<score-item :score="0" :itemid="info.id" :comment="'阅读文章：' + info.title"></score-item>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				info: {}
			}
		},
		onLoad(e) {

			this.gethome(e.id)
		},
		onShow() {
			
		},
		methods: {
			gethome(id) {
				uni.$u.http.post('Info/byData', {id:id}).then(res => {
					this.info = res.data;
					
				})
			},
			
			
		},
		
	}
</script>

<style>
</style>