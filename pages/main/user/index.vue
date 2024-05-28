<template>
  <view class="user">
    <!-- 头部 -->
    <view class="user-wrap">
      <view class="setting">
        <u-icon name="setting" color="#FFF" size="28" @click="user()"></u-icon>
      </view>
      <view class="info">
        <image class="avatar" mode="aspectFill" :src="userInfo.headPicUrl"></image>
        <view class="nickname">{{ userInfo.nickName }}</view>
        <view class="score_card" @click="handleUseScore">
          <u-icon size="20" name="integral" color="#FFD700" />
          积分:
          {{ userInfo.score }}
        </view>
      </view>
    </view>

    <view class="order-status">
      <view class="status-wrap">
        <view class="cell">
          <view class="cell-left">
            <image
              class="cell-icon"
              src="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/gongju.png"
              mode="aspectFill"
            ></image>
            <view class="cell-text">我的工具</view>
          </view>
        </view>
        <view class="status-list">
          <view class="status-item" hover-class="btn-hover" v-for="(item, index) in orderStatusList" :key="index">
            <image mode="widthFix" :src="item.icon" style="width: 50%" @click="Jom(item.url)"></image>
            <view class="item-text">{{ item.name }}</view>
          </view>
        </view>
      </view>
    </view>

    <view class=".order-status-gj" v-if="workinfo&&workinfo.length > 0">
      <view class="status-wrap">
        <view class="cell">
          <view class="cell-left">
            <image
              class="cell-icon"
              src="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/gongzuo.png"
              mode="aspectFill"
            ></image>
            <view class="cell-text">工作台</view>
          </view>
        </view>
        <view class="status-list">
          <view class="status-item" hover-class="btn-hover" v-for="(item, index) in StatusList" :key="index">
            <image mode="widthFix" :src="item.icon" style="width: 50%" @click="jon(item.url)"></image>
            <view class="item-text">{{ item.name }}</view>
          </view>
        </view>
      </view>
    </view>

    <!-- <view class="com-item">
		<view class="com-wrap">
			<view class="cell" v-for="(item, index) in orderStatusList1" :key="index"  @click="Jom(item.url)">
				<view class="cell-left">
					<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
					<view class="cell-text">{{ item.name }}</view>
				</view>
				<view class="iconfont iconmore1"></view>
			</view>
		</view>
	</view> -->

    <!-- 用户服务 -->
    <view class="com-item">
      <view class="com-wrap">
        <view class="cell" v-for="(item, index) in serverList" :key="index" @click="jon(item.path)">
          <view class="cell-left">
            <image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
            <view class="cell-text">{{ item.title }}</view>
          </view>
          <view class="iconfont iconmore1"></view>
        </view>
      </view>
    </view>
    <view class="com-item">
      <view class="com-wrap">
        <!-- <view class="cell" @click="jon('/pages/componentsC/about/index')">
          <view class="cell-left">
            <image
              class="cell-icon"
              src="/static/images/hezuo.png"
              mode="aspectFill"
            ></image>
            <view class="cell-text">技术支持与合作</view>
          </view>
          <view class="iconfont iconmore1"></view>
        </view> -->
        <view class="cell">
          <view class="cell-left">
            <image class="cell-icon" src="/static/images/icon-collect.png" mode="aspectFill"></image>
            <view class="cell-text">系统版本号：1.0.2</view>
          </view>
          <!-- <view class="iconfont iconmore1"></view> -->
        </view>
      </view>
    </view>
    <u-modal :show="useScoreStatus.showModal" :closeOnClickOverlay="true" @close="useScoreStatus.showModal = false"
     title="提示" @confirm="handleConfirmUseScore">
      <view class="slot-content w-full">
        <view class="modal_content">
          <view>请输入要核销的积分数额</view>
          <!-- <view @click="showScoreUser = true" class="flex justify-between w-full">
            <view>核销人</view>
            <view class="flex-center gap-6">
              <view v-if="useScoreStatus.name">{{useScoreStatus.name}}</view>
              <view style="color: #007aff" v-else>请选择</view>
              <u-icon name="arrow-right" size="16" />
            </view>
            <u-picker :show="showScoreUser" :columns="[scoreUserList]" keyName="label" @confirm="onChangeScoreUser" @cancel="showScoreUser = false"

             />
          </view> -->
          <u-cell-group customStyle="width:100%;">
            <u-cell title="积分数额">
              <u-number-box v-model="useScoreStatus.score" min="0" :max="userInfo.score" slot="right-icon" />
            </u-cell>
            <u-picker
              :show="showScoreUser"
              :columns="[scoreUserList]"
              keyName="label"
              @confirm="onChangeScoreUser"
              @cancel="showScoreUser = false"
            />
            <u-cell title="核销人" isLink :value="useScoreStatus.name" @click="showScoreUser = true" />
          </u-cell-group>
        </view>
      </view>
    </u-modal>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        workinfo: [],
        userInfo: {
          headPicUrl: "",
          nickName: "",
          score: 0
        },
        useScoreStatus: {
          showModal: false,
          score: 0,
          itemid: 0,
          name: ""
        },
        scoreUserList: [],
        showScoreUser: false,
        orderStatusList: [
          // { name: '我的成员', icon: 'https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/shi.png', status: 50 },
          {
            name: "我的维修",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/xiu.png",
            url: "/pages/componentsA/baoxiu/list"
          },
          {
            name: "我的报名",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/huodong.png",
            url: "/pages/componentsB/activity/userList"
          },
          {
            name: "我的求助",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/bang.png",
            url: "/pages/componentsA/bangzhu/list"
          },
          {
            name: "我的置物",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/zhiwu.png",
            url: "/pages/componentsC/storage/storage"
          }
        ],
        StatusList: [
           {
            name: "数据总览",
            icon: "https://ys-village.oss-cn-shanghai.aliyuncs.com/xcx/shuju.png",
            url: "/pages/componentsA/shuju/shuju"
          },
          {
            name: "维修工单",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/xiu.png",
            url: "/pages/componentsA/baoxiu/gongdan"
          },
          {
            name: "投诉处理",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/jianyi.png",
            url: "/pages/main/suggestion/gdlist"
          },
          {
            name: "积分核销",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/xunjian.png",
            url: "/pages/componentsA/score/record"
          },
          {
            name: "联系住户",
            icon: "https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/xia/anfang.png",
            url: "/pages/componentsA/security/security"
          }
        ],

        currentIndex: 0,

        serverList: [
          {
            title: "对公账户",
            icon: "/static/images/class-02.png",
            path: "/pages/componentsC/corporate/corporate"
          },

          {
            title: "用户协议",
            icon: "/static/images/icon-kefu.png",
            path: "/pages/componentsC/agreement/agreement"
          },
          {
            title: "隐私政策",
            icon: "/static/images/icon-about.png",
            path: "/pages/componentsC/policy/policy"
          },
          {
            title: "帮助中心",
            icon: "/static/images/icon-help.png",
            path: "/pages/componentsC/help/help"
          }
        ]
      }
    },
    onLoad() {
      uni.hideShareMenu()
      uni.onCopyUrl((result) => {
        setTimeout(() => {
          uni.setClipboardData({
            data: "不可复制哦"
          })
        }, 1000)
      })
    },
    onShow() {
      const userData = uni.getStorageSync("userData")
      if (!userData) {
        // uni.$u.route({
        //   url: '/pages/componentsA/login/index'
        // });
        return
      } else {
        this.workinfo = userData.workinfo
        this.userInfo = {
          headPicUrl: userData.ioc,
          nickName: userData.name,
          score: 0
        }
      }
      this.getScore()
    },
    methods: {
      handleUseScore() {
        ;(this.useScoreStatus = {
          showModal: true,
          score: 0,
          itemid: 0,
          name: ""
        }),
          this.getScoreUsers()
      },
      handleConfirmUseScore() {
        if (this.useScoreStatus.score == 0) {
          uni.showToast({
            title: "请输入积分数额",
            icon: "none"
          })
          return
        }
        if (!this.useScoreStatus.name) {
          uni.showToast({
            title: "请选择核销人",
            icon: "none"
          })
          return
        }
        uni.$u.http
          .post("ScoreRecord/add", {
            wxid: uni.getStorageSync("userData").uid,
            score: -this.useScoreStatus.score,
            comment: '积分核销：' + this.useScoreStatus.name,
            ctime: new Date().getTime(),
            itemid: this.useScoreStatus.itemid
          })
          .then((res) => {
            uni.showToast({
              title: "核销成功",
              icon: "none"
            })
            this.getScore()
            this.useScoreStatus.showModal = false
          })
      },

      onChangeScoreUser(e) {
        console.log(e)
        this.useScoreStatus.name = e.value[0].label
        this.useScoreStatus.itemid = e.value[0].id
        this.showScoreUser = false
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
      user() {
        uni.$u.route({
          url: "/pages/componentsC/user/user"
        })
      },
      jon(path) {
        // console.log(path)
        uni.$u.route({
          url: path
        })
      },
      getScore() {
        uni.$u.http
          .post("ScoreRecord/getUserTotalScore", {
            wxid: uni.getStorageSync("userData").uid
          })
          .then((res) => {
            this.userInfo.score = res.data
          })
      },
      getScoreUsers() {
        uni.$u.http.post("AdminUser/scoreuserlist").then((res) => {
          this.scoreUserList = res.data.map((item) => {
            return {
              label: item.name + "-(" + item.zw + ")",
              id: item.id
            }
          })
          console.log(this.scoreUserList)
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  page {
    background: #f2f2f2;
  }
  .btn-hover {
    background: #f2f2f2 !important;
  }
  .modal_content {
    display: flex;
    flex-direction: column;
    gap: 24rpx;
    align-items: center;
    justify-content: center;
  }
</style>
