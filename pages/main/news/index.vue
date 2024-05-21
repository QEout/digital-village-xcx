<template>
  <div>
    <u-sticky bgColor="#fff">
      <u-tabs :list="tabs" itemStyle="width:40%; height: 42px;" @change="changeTab"></u-tabs>
    </u-sticky>
    <!-- 通知消息 -->
    <view v-if="currentIndex === 0">
      <view v-if="list.length > 0">
        <view class="body_gong" v-for="(item, index) in list" :key="index">
          <view class="fu_title">{{ item.title }} </view>
          <u-line margin="20rpx 0rpx"></u-line>
          <view class="time">
            <u-icon
              :label="item.ctime"
              size="20"
              labelSize="14"
              labelColor="#747474"
              name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/time.png"
            ></u-icon>
          </view>
        </view>
      </view>
      <view v-if="list.length == 0">
        <u-empty
          marginTop="100"
          mode="message"
          icon="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/xiaoxi.png"
        >
        </u-empty>
      </view>
    </view>
    <!-- 积分消息 -->
    <view v-if="currentIndex === 1">
      <view v-if="list.length > 0" style="width: 100%; padding: 20rpx; box-sizing: border-box">
        <!-- <view class="body_gong" v-for="(item,index) in list" :key="index">
        <view class="fu_title">{{item.title}} </view>
        <u-line margin="20rpx 0rpx"></u-line>
        <view class="time">
          <u-icon :label="item.ctime" size="20" labelSize="14" labelColor="#747474" name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/time.png"></u-icon>
        </view>
      </view> -->
        <u-list @scrolltolower="scrolltolower" height="calc(100vh - 90px)">
          <u-list-item v-for="(item, index) in scoreList" :key="index">
            <view class="score_card">
              <view class="score_left">
                <view class="score_title">
                  {{ item.comment }}
                </view>
                <view class="score_time">
                  {{ item.ctime }}
                </view>
              </view>
              <view :class="item.score > 0 ? 'green_text' : 'red_text'">
                {{ item.score > 0 ? "+" + item.score : item.score }}
              </view>
            </view>
          </u-list-item>
        </u-list>
        <u-loadmore :status="pagination.hasMore ? 'loading' : 'nomore'"></u-loadmore>
      </view>
      <view v-if="list.length == 0">
        <u-empty
          marginTop="100"
          mode="message"
          icon="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/xiaoxi.png"
        >
        </u-empty>
      </view>
    </view>
  </div>
</template>

<script>
  import uDivider from "../../../uni_modules/uview-ui/components/u-divider/u-divider.vue"
  export default {
    components: { uDivider },
    data() {
      return {
        list: [],
        scoreList: [],
        pagination: {
          page: 1,
          limit: 20,
          hasMore: true
        },
        tabs: [
          {
            name: "通知消息"
          },
          {
            name: "积分消息"
          }
        ],
        currentIndex: 0
      }
    },
    onLoad() {
      this.gethome()
    },
    onShow() {},
    methods: {
      gethome(id) {
        uni.$u.http.post("News/list", { wxid: uni.getStorageSync("userData").uid }).then((res) => {
          this.list = res.data
        })
      },
      getScore() {
        uni.$u.http
          .post("ScoreRecord/list", {
            wxid: uni.getStorageSync("userData").uid,
            page: this.pagination.page,
            noZero: true,
            limit: this.pagination.limit
          })
          .then((res) => {
            if (res.data.list.length < this.pagination.limit) {
              this.pagination.hasMore = false
            }
            this.scoreList.push(...res.data.list)
          })
      },
      scrolltolower() {
        if (this.pagination.hasMore) {
          this.pagination.page++
          this.getScore()
        }
      },
      changeTab(e) {
        this.currentIndex = e.index
        if (e.index === 1) {
          this.pagination = {
            page: 1,
            limit: 20,
            hasMore: true
          }
          this.scoreList = []
          this.getScore()
        } else {
          this.gethome()
        }
      }
    }
  }
</script>

<style>
  .score_card {
    padding: 20rpx;
    box-sizing: border-box;
    border-radius: 10rpx;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #fff;
    box-shadow: 0 0 10rpx 0 rgba(0, 0, 0, 0.1);
  }
  .score_left {
    display: flex;
    line-height: 28rpx;
    flex-direction: column;
    gap: 20rpx;
  }

  .score_title {
    font-size: 28rpx;
    color: #333;
  }
  .score_time {
    font-size: 24rpx;
    color: #747474;
  }
  .green_text {
    font-size: 28rpx;
    color: #3cb371;
  }
  .red_text {
    font-size: 28rpx;
    color: #ff6347;
  }
</style>
