<template>
  <view>
    <u-swiper :list="list" height="180" :circular="true" radius="0"
      @click="previewImage"
    ></u-swiper>
    <u-notice-bar
      icon="http://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/gonggao.png"
      :text="text4"
      direction="column"
      color="#7b7979"
      bgColor="#fff"
      fontSize="15"
      url="/pages/componentsB/notice/list"
    >
    </u-notice-bar>
    <view class="bankuai">
      <view class="biaoti">
        <u-icon label="常用服务" size="20" name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/shu.png"></u-icon>
      </view>
      <u-row justify="space-between" customStyle="margin: 10px 0 0 0" gutter="10">
        <u-col span="6">
          <view class="demo-layout-a" @click="Jom('/pages/componentsA/fangwu/details')">
            <image src="../../../static/images/fangwu.png" mode="widthFix"></image>
          </view>
        </u-col>
        <u-col span="6">
          <view class="demo-layout-a" @click="Jom('/pages/componentsA/baoxiu/list')">
            <image src="../../../static/images/baoxiu.png" mode="widthFix"></image>
          </view>
        </u-col>
      </u-row>
      <u-row justify="space-between" customStyle="margin: 10px 0 0 0" gutter="10">
        <u-col span="6">
          <view class="demo-layout-a" @click="Jom('/pages/componentsA/che/details')">
            <image src="../../../static/images/chewei.png" mode="widthFix"></image>
          </view>
        </u-col>
        <u-col span="6">
          <view class="demo-layout-a" @click="Jom('/pages/componentsA/bill/bill')">
            <image src="../../../static/images/jiaofei.png" mode="widthFix"></image>
          </view>
        </u-col>
      </u-row>
    </view>
    <view class="bankuai" style="padding-bottom: 0px" v-if="list2.length > 0">
      <view class="biaoti">
        <u-icon label="快捷联系" size="20" name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/shu.png"></u-icon>
      </view>
      <view class="xiabiao pb-0">
        <u-scroll-list :indicator="false">
          <view class="scroll-list" style="flex-direction: row">
            <view
              class="person_item"
              v-for="(item, index) in list2"
              :key="index"
              :class="[index === 9 && 'scroll-list__goods-item--no-margin-right']"
            >
              <image class="person_item_image" :src="item.ioc" mode=""></image>

              <text class="person_item_text">{{ item.zw }}</text>
              <text class="person_item_name">{{ item.name }}</text>
              <text class="person_item_phone" @click="bo(item.phone)"> 立即联系 </text>
            </view>
          </view>
        </u-scroll-list>
      </view>
    </view>
    <view class="bankuai" v-if="huodonglist.length">
      <view class="biaoti">
        <u-icon label="活动" size="20" name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/shu.png"></u-icon>
      </view>
      <view
        class="biankuang"
        v-for="(itme, index) in huodonglist"
        :key="index"
        @click="Jom('/pages/componentsB/activity/details?id=' + itme.id)"
      >
        <image :src="itme.ioc" mode="widthFix" class="huodongimage"></image>
        <view class="huodongtitle">{{ itme.title }}</view>
        <view class="huodongtitle"
          ><text>活动名额：{{ itme.rmun }}名</text></view
        >
        <view class="huodongtitle">报名开始时间：{{ itme.stime }}</view>
        <view class="huodongtitle">报名结束时间：{{ itme.etime }}</view>
        <view class="huodongtitle"
          ><text>报名状态：{{ itme.zt == 1 ? "进行中" : itme.zt == 3 ? "结束" : "未开始" }}</text></view
        >
      </view>
    </view>
    <view class="bankuai" style="padding-top:0px;">
      <view>
        <view class="hend">
          <u-tabs :list="tablist" keyName="title" @click="clickTab"></u-tabs>
        </view>
        <view class="body_gong" v-for="(o, index) in articlelist" :key="index" @click="xq(o.id)">
          <image :src="o.ioc" mode="widthFix" style="width: 100%"></image>
          <!-- <u-line margin="20rpx 0rpx"></u-line> -->
          <view class="title">{{ o.title }} </view>
          <view class="time">
            <u-icon
              :label="o.ctime"
              size="20"
              labelSize="14"
              labelColor="#747474"
              name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/time.png"
            ></u-icon>
          </view>
        </view>
        <view v-if="articlelist.length == 0">
          <u-empty
            marginTop="100"
            text="暂无数据"
            mode="message"
            icon="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/nei.png"
          >
          </u-empty>
        </view>
        <u-loadmore
          :status="status"
          v-if="articlelist.length > 0"
          loadingText="努力加载中,先喝杯茶"
          :line="true"
          marginBottom="30"
        />
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        current: 0,
        currentNum: 0,
        list: [],
        text4: [],
        articlelist: [],
        page: 1,
        xia: 0,
        info_class_id: 0,
        tablist: [],
        list2: [],
        huodonglist: []
      }
    },
    onShow() {
      this.gethome()
      this.list = []
      this.articlelist = 0
      this.page = 1
      this.getlx()
    },
    onReachBottom() {
      if (this.xia == 0) {
        this.gethome()
      }
    },
    methods: {
      previewImage(e) {
        uni.previewImage({
          current: e,
          urls: this.list
        })
      },
      gethome() {
        uni.$u.http.post("Carousel/xcxList", {}).then((res) => {
          this.list = res.data
        })
        uni.$u.http
          .post("Notice/xcxlist", {
            aid: 0,
            name: "",
            page: 1,
            limit: 5
          })
          .then((res) => {
            var arr = []
            for (let i = 0; i < res.data.length; i++) {
              arr.push(res.data[i].title)
            }
            this.text4 = arr
          })
        uni.$u.http.post("AdminUser/xcxlist", {}).then((res) => {
          this.list2 = res.data
        })
        uni.$u.http.post("Activity/xcxlist", { lx: 0, page: 1, limit: 10 }).then((res) => {
          this.huodonglist = res.data
        })
      },
      bo(e) {
        uni.makePhoneCall({
          phoneNumber: e, //电话号码
          success: function (e) {
            console.log(e)
          },
          fail: function (e) {
            console.log(e)
          }
        })
      },
      Jom(path) {
        let houses = uni.getStorageSync("userData").houses
        if (houses == 0) {
          uni.showModal({
            title: "提示",
            content: "请先进行认证",
            success: function (res) {
              if (res.confirm) {
                uni.$u.route({
                  url: "/pages/componentsA/fangwu/add"
                })
              } else if (res.cancel) {
                console.log("用户点击取消")
              }
            }
          })
        } else {
          uni.$u.route({
            url: path
          })
        }
      },
      getlx() {
        uni.$u.http.post("InfoClass/xllist", {}).then((res) => {
          this.tablist = res.data
          this.info_class_id = res.data[0].id
          this.articlelist = []
          this.xia = 0
          this.page = 1
          this.getArticleList()
        })
      },
      clickTab(item) {
        this.info_class_id = item.id
        this.articlelist = []
        this.xia = 0
        this.page = 1
        this.getArticleList()
      },
      getArticleList() {
        uni.$u.http
          .post("Info/xcxlist", { info_class_id: this.info_class_id, page: this.page, limit: 10 })
          .then((res) => {
            this.articlelist = this.articlelist.concat(res.data)
            if (res.data.length > 9) {
              this.page = this.page + 1
            } else {
              this.xia = 1
              this.status = "nomore"
              this.loadingText = "无更多数据"
            }
          })
      },
      xq(id) {
        uni.$u.route({
          url: "/pages/componentsB/info/details?id=" + id
        })
      }
    }
  }
</script>

<style scoped>
  .person_item {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-right: 24rpx;
    width: 200rpx;
    margin-bottom: 20rpx;
    border-radius: 10rpx;
    background-color: #fff;
    padding: 18rpx;
    box-shadow: 4rpx 4rpx 20rpx rgba(0, 0, 0, 0.1);
  }

  .person_item_image {
    width: 120rpx;
    height: 120rpx;
    border-radius: 50%;
    object-fit: cover;

  }

  .person_item_text {
    font-size: 32rpx;
    color: #333;
    margin-top: 20rpx;
  }

  .person_item_name {
    font-size: 32rpx;
    color: #666;
    margin-top: 10rpx;
  }

  .person_item_phone {
    font-size: 28rpx;
    color: #2b85e4;
    margin-top: 10rpx;
    cursor: pointer;
  }
</style>
