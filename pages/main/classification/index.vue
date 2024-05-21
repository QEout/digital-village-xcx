<template>
  <view>
    <view class="bankuai" v-for="(item, index) in list" :key="index">
      <view class="biaoti">
        <u-icon
          :label="item.title"
          size="20"
          name="https://shopimges.oss-cn-hangzhou.aliyuncs.com/wuye/shu.png"
        ></u-icon>
      </view>
      <view>
        <u-grid :border="false" col="4">
          <u-grid-item
            v-for="(listItem, listIndex) in item.list"
            :key="listIndex"
            customStyle="padding-top: 10px; padding-bottom: 10px"
            @click="Jom(listItem)"
          >
            <u-icon :name="listItem.ioc" :size="40"></u-icon>
            <text class="grid-text">{{ listItem.name }}</text>
          </u-grid-item>
        </u-grid>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        list: []
      }
    },
    onLoad() {},
    onShow() {
      this.gethome()
    },
    methods: {
      gethome() {
        uni.$u.http.post("Menu/list", {}).then((res) => {
          let tempList = res.data
          if (
            !uni.getStorageSync("userData").workinfo ||
            uni.getStorageSync("userData").workinfo.filter((item) => item.cname == "CPC").length == 0
          ) {
            tempList = tempList.filter((item) => item.title != "智慧党建")
          }
          this.list = tempList
        })
      },
      Jom(itme) {
        if (itme.aid == 1) {
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
              url: itme.url
            })
          }
        } else {
          uni.$u.route({
            url: itme.url
          })
        }
      }
    }
  }
</script>

<style></style>
