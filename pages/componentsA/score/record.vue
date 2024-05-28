<template>
  <view>
    <u-sticky>
      <view class="flex-col p-4" style="background: white; border-top: 1px solid #f4f4f4">
        <view class="flex-row gap-4 mb-4 items-center justify-center" @click="showPicker = true">
          <view >{{ formatTime }}</view>
          <u-icon name="calendar" size="20" color="#606266"></u-icon>
        </view>
        <view class="mx-auto text-sm text-low"
        >共核销积分：{{ -totalScore }}</view>
      </view>
    </u-sticky>
    <u-datetime-picker
      :show="showPicker"
      v-model="date"
      ref="picker"
      mode="year-month"
      @confirm="confirm"
      @cancel="showPicker = false"
    />
    <view class="p-8">
      <u-list @scrolltolower="scrolltolower" v-if="list.length > 0">
        <u-list-item v-for="(item, index) in list" :key="index" >
          <view class="flex justify-between score-card">
            <!-- wx_name,wx_ioc -->
            <view class="flex items-center gap-4">
              <image :src="item.wx_ioc" class="score-avatar"></image>
              <view class="flex-col gap-2">
                <view>{{ item.wx_name }}</view>
                <view class="text-low text-sm">{{ item.ctime }}</view>
              </view>
            </view>
            <view class="score-val">{{ item.score }}</view>
          </view>
        </u-list-item>
        <u-loadmore :status="pagination.hasMore ? 'loading' : 'nomore'"></u-loadmore>
      </u-list>
      <view v-if="list.length == 0">
        <u-empty
          marginTop="100"
          text="暂无数据"
          mode="message"
          icon="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/nei.png"
        >
        </u-empty>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        date: new Date(),
        formatTime: uni.$u.timeFormat(new Date(), "yyyy-mm"),
        pagination: {
          page: 1,
          limit: 20,
          hasMore: true
        },
        list: [],
        showPicker: false,
        totalScore: 0
      }
    },
    onShow() {
      this.gethome()
    },
    methods: {
      confirm(e) {
        this.formatTime = uni.$u.timeFormat(new Date(e.value), "yyyy-mm")
        this.showPicker = false
        this.pagination.page = 1
        this.list = []
        this.gethome()
      },
      gethome() {
        uni.$u.http
          .post("ScoreRecord/list", {
            itemid: uni.getStorageSync("userData").workinfo.find((item) => item.did==1).id,
            date: this.formatTime,
            page: this.pagination.page,
            limit: this.pagination.limit
          })
          .then((res) => {
            this.list = this.list.concat(res.data.list)
            if (res.data.list.length < this.pagination.limit) {
              this.pagination.hasMore = false
            }
            this.totalScore = res.data.totalScore
          })
      },
      scrolltolower() {
        if (this.pagination.hasMore) {
          this.pagination.page++
          this.gethome()
        }
      }
    }
  }
</script>

<style>
  .score-card {
    padding: 16rpx 20rpx;
    margin-bottom: 16rpx;
    background: white;
    border-radius: 10rpx;
  }
  .score-avatar {
    width: 80rpx;
    height: 80rpx;
    border-radius: 10rpx;
  }
  .score-val {
    font-size: 48rpx;
    color: #40d140;
  }
</style>
