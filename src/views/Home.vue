<template>
  <div>
    <TikTokVideo v-if="tabIndex === 0"></TikTokVideo>
    <collectVideo v-if="tabIndex === 2"></collectVideo>
    <!--底部操作栏-->
    <div class="container_bottom">
      <div class="border_progress" :style="'width:' + videoProcess + '%'"></div>
      <div
        class="bottom_tab"
        :class="tabIndex == 0 ? 'tab_active' : ''"
        @click="changeTab(0)"
      >
        <span class="bottom_tab_span ">首页</span>
      </div>
      <div
        class="bottom_tab"
        :class="tabIndex == 1 ? 'tab_active' : ''"
        @click="changeTab(1)"
      >
        <img
          src="http://oss.jishiyoo.com/images/file-1575427746903.png"
          alt=""
          class="bottom_tab_icon"
        />
      </div>
      <div
        class="bottom_tab"
        :class="tabIndex == 2 ? 'tab_active' : ''"
        @click="changeTab(2)"
      >
        <span class="bottom_tab_span">视频聚合 </span>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import { Swipe, SwipeItem, Toast } from "vant";
import TikTokVideo from "./TikTokVideo.vue";
import collectVideo from "./collectVideo.vue";
// 引入微信分享
import wx from "weixin-js-sdk";
Vue.use(Swipe, Toast).use(SwipeItem);

let videoProcessInterval;
export default {
  name: "home",
  components: {
    collectVideo,
    TikTokVideo,
  },
  data() {
    let u = navigator.userAgent;
    return {
      current: 0,

      isVideoShow: true,
      playOrPause: true,
      video: null,
      iconPlayShow: true,
      isAndroid: u.indexOf("Android") > -1 || u.indexOf("Adr") > -1, // android终端
      isiOS: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), // ios终端
      tabIndex: 0,
      showShareBox: false, //展示分享框
      commentPop: false, //是否展示评论弹框
      // 评论相关
      comment_text: "", //评论输入内容
      canSend: false, //是否可以发送
      videoComment: [],
      commentPlaceholder: "留下你精彩的评论吧", //评论Placeholder
      replayUserData: "",
      to_comment_id: "",
      videoProcess: 0, //视频播放进度
    };
  },
  watch: {},
  mounted() {},
  methods: {
    //改变菜单
    changeTab(index) {
      this.tabIndex = index;
    },
  },
};
</script>
<style scoped>
.container_bottom {
  position: fixed;
  bottom: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.85);
  height: 48px;
  /*border-top: 1px solid rgba(255, 255, 255, 0.7);*/
  max-width: 550px;
}

.border_progress {
  width: 0;
  height: 1px;
  background: #bababa;
  position: absolute;
  top: 0;
  transition: 0.1s all;
}

.bottom_tab {
  width: 33%;
  text-align: center;
  float: left;
  color: #c9c9c9;
  padding: 10px 0;
}

.bottom_tab_icon {
  width: 44px;
}

.bottom_tab_span {
  line-height: 28px;
  border-bottom: 2px solid transparent;
  font-weight: 600;
  font-size: 16px;
  display: inline-block;
  padding: 0 4px;
  transition: 0.5s all;
}

.tab_active .bottom_tab_span {
  border-bottom: 2px solid #fff;
  color: #fff;
}
</style>
