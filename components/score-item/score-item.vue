<template>
	<view>
		<view class="score_item" v-if="score > 0 && !isRead">
			<u-icon size="20" name="integral" color="#FFD700" />
			{{ score }}
		</view>
		<view v-if="isRead" style="color: #999;"> 已读 </view>
	</view>
</template>
<script>
	export default {
		props: {
			score: {
				type: Number,
			},
			itemid: {
				type: Number,
			},
			comment: {
				type: String,
				default: "",
			},
			readable: {
				type: Boolean,
				default: true,
			},
		},
		data() {
			return {
				isRead: false,
			};
		},
		watch: {
			itemid: {
				handler: function(val) {
					if (val && this.readable) {
						this.isRead = false;
						this.addReadRecord();
					}
				},
				immediate: true,
			},
		},
		methods: {
			addReadRecord() {
				if (!this.itemid) return;
				uni.$u.http.post("ScoreRecord/add", {
						wxid: uni.getStorageSync("userData").uid,
						itemid: this.itemid,
						score: this.score,
						ctime: new Date().getTime(),
						comment: this.comment,
					})
					.then((res) => {
						if (res.msg === "success") {
							uni.showToast({
								title: "积分+" + this.score,
								icon: "none",
							});
						}
						this.isRead = true;
					});
			},
		},
	};
</script>