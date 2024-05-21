<template>
	<view style="padding-bottom: 100rpx">
		<view class="body_gong" v-if="notice">
			<view class="title_jz">{{ notice.title }}
			</view>
			<u-line margin="20rpx 0rpx"></u-line>
			<view class="">
				<u-parse :content="notice.content"></u-parse>
			</view>
			<view class="time">
				<u-icon :label="notice.ctime" size="20" labelSize="14" labelColor="#747474"
					name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/time.png"></u-icon>
				<score-item :score="notice.score" :itemid="notice.id" :comment="'阅读公告：' + notice.title"></score-item>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				notice: {},
			};
		},
		onLoad(e) {
			this.gethome(e.id);
		},
		onShow() {},
		methods: {
			gethome(id) {
				uni.$u.http.post("Notice/byData", {
					id: id
				}).then((res) => {
					this.notice = res.data;
				});
			}
		},
	};
</script>

<style>
</style>