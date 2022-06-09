<template>
  <div class="container">
    <div v-if="isShowVideo">
      <video
        style="width: 100%; height:100vh;object-fit:fill"
        loop
        webkit-playsinline="true"
        x5-video-player-type="h5-page"
        x5-video-player-fullscreen="true"
        playsinline
        preload="auto"
        :src="videoUrl"
        @click="pauseVideo(current)"
        :playOrPause="playOrPause"
      ></video>
      <img
        v-show="iconPlayShow"
        class="icon_play"
        @click="playvideo(index)"
        src="../static/video/icon_play.png"
      />
    </div>

    <mescroll-vue
      v-else
      ref="mescroll"
      :down="mescrollDown"
      :up="mescrollUp"
      @init="mescrollInit"
    >
      <div v-for="(item, index) in dataList" :key="item.id" class="box">
        <!-- <video
          style="width: 100%; height:90%;object-fit:fill"
          loop
          webkit-playsinline="true"
          x5-video-player-type="h5-page"
          x5-video-player-fullscreen="true"
          playsinline
          preload="auto"
          :poster="item.cover"
          :src="item.url"
          @click="pauseVideo(index)"
          :playOrPause="playOrPause"
        ></video> -->
        <img
          :src="item.cover"
          style="width: 100%; height:100%;"
          @click="showvideo(index)"
        />
        <div class="myicon">
          <van-icon name="like-o" size="18px" color="white" />
          <span style="margin-left:5px;">12.3W</span>
        </div>

        <!-- 播放暂停按钮 -->
        <!-- <img
          v-show="allPause ? allPause : item.isPause"
          class="icon_play"
          @click="playvideo(index)"
          src="http://npjy.oss-cn-beijing.aliyuncs.com/images/file-1575340653940esdHR.png"
        /> -->
      </div>
    </mescroll-vue>
  </div>
</template>

<script>
import MescrollVue from "mescroll.js/mescroll.vue";
let videoProcessInterval;
export default {
  components: {
    MescrollVue, // 注册mescroll组件
  },
  data() {
    let u = navigator.userAgent;
    return {
      isShowVideo: false,
      allPause: true,
      current: 0,
      isVideoShow: true,
      videoProcess: 0, //视频播放进度
      video: null,
      iconPlayShow: true,
      isAndroid: u.indexOf("Android") > -1 || u.indexOf("Adr") > -1, // android终端
      isiOS: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), // ios终端
      playOrPause: true,
      mescroll: null, // mescroll实例对象
      mescrollDown: {}, //下拉刷新的配置. (如果下拉刷新和上拉加载处理的逻辑是一样的,则mescrollDown可不用写了)
      mescrollUp: {
        // 上拉加载的配置.
        callback: this.upCallback, // 上拉回调,此处简写; 相当于 callback: function(page, mescroll) { }
        //以下是一些常用的配置,当然不写也可以的.
        page: {
          num: 0, //当前页 默认0,回调之前会加1; 即callback(page)会从1开始
          size: 10, //每页数据条数,默认10
        },
        htmlNodata: '<p class="upwarp-nodata">-- END --</p>',
        noMoreSize: 5, //如果列表已无数据,可设置列表的总数量要大于5才显示无更多数据;避免列表数据过少(比如只有一条数据),显示无更多数据会不好看	这就是为什么无更多数据有时候不显示的原因
        toTop: {
          //回到顶部按钮
          src: "./static/mescroll/mescroll-totop.png", //图片路径,默认null,支持网络图
          offset: 1000, //列表滚动1000px才显示回到顶部按钮
        },
        empty: {
          //列表第一页无任何数据时,显示的空提示布局; 需配置warpId才显示
          warpId: "xxid", //父布局的id (1.3.5版本支持传入dom元素)
          icon: "./static/mescroll/mescroll-empty.png", //图标,默认null,支持网络图
          tip: "暂无相关数据~", //提示
        },
      },
      videoList: [],
      videoUrl: "",
      dataList: [
        {
          id: 0,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
          isVideo: true, //判断是否是视频
        },
        {
          id: 1,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
        {
          id: 2,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
        {
          id: 3,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
        {
          id: 4,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
        {
          id: 5,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
        {
          id: 6,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
        {
          id: 7,
          url:
            "https://stream7.iqilu.com/10339/upload_transcode/202002/18/20200218093206z8V1JuPlpe.mp4", //视频源
          cover: "http://oss.jishiyoo.com/images/file-1575341210559.png",
          des: "武汉加油",
          isPause: true,
        },
      ], // 列表数据
    };
  },
  methods: {
    // mescroll组件初始化的回调,可获取到mescroll对象
    mescrollInit(mescroll) {
      this.mescroll = mescroll; // 如果this.mescroll对象没有使用到,则mescrollInit可以不用配置
    },
    // 上拉回调 page = {num:1, size:10}; num:当前页 ,默认从1开始; size:每页数据条数,默认10
    upCallback(page, mescroll) {
      // 联网请求
      // axios
      //   .get("xxxxxx", {
      //     params: {
      //       num: page.num, // 页码
      //       size: page.size, // 每页长度
      //     },
      //   })
      //   .then((response) => {
      //     // 请求的列表数据
      //     let arr = response.data;
      //     // 如果是第一页需手动置空列表
      //     if (page.num === 1) this.dataList = [];
      //     // 把请求到的数据添加到列表
      //     this.dataList = this.dataList.concat(arr);
      //     // 数据渲染成功后,隐藏下拉刷新的状态
      //     this.$nextTick(() => {
      //       mescroll.endSuccess(arr.length);
      //     });
      //   })
      //   .catch((e) => {
      //     // 联网失败的回调,隐藏下拉刷新和上拉加载的状态;
      //     mescroll.endErr();
      //   });
    },
    showvideo(index) {
      this.current = index;
      this.isShowVideo = true;
      this.videoUrl = this.dataList[index].url;
    },
    // 开始播放
    playvideo(event) {
      this.allPause = false;
      // for (var i = 0; i < this.dataList.length; i++) {
      //   if (event === i) {
      //     this.dataList[i].isPause = !this.dataList[i].isPause;
      //   } else {
      //     this.dataList[i].isPause = true;
      //   }
      // }
      this.current = event;
      //this.iconPlayShow = event;
      console.log("event", event);
      let video = document.querySelectorAll("video")[0];
      console.log("playvideo：" + this.current);
      this.isVideoShow = false;
      this.iconPlayShow = false;
      this.showShareBox = false;
      this.playOrPause = true;
      video.play();

      if (this.isiOS) {
        setTimeout(() => {
          //处理ios宽视频
          let documentW =
            document.documentElement.clientWidth || document.body.clientWidth;
          let docB = parseFloat(video.videoWidth / documentW);
          console.log("获取视频宽和屏幕比：" + docB);
          // 计算视频最适高度
          let realH = parseInt(video.videoHeight / docB) + 100;
          this.realH = realH + "px";
          console.log("视频最适高度：" + this.realH);
          this.$forceUpdate();
        }, 200);
      }

      videoProcessInterval = setInterval(() => {
        this.changeProcess(video);
      }, 100);
    },
    pauseVideo(id) {
      //this.isShowVideo = !this.isShowVideo;
      this.allPause = false;

      // for (var i = 0; i < this.dataList.length; i++) {
      //   if (id === i) {
      //     this.dataList[i].isPause = !this.dataList[i].isPause;
      //   } else {
      //     this.dataList[i].isPause = true;
      //   }
      // }

      //暂停\播放

      this.current = id;
      //this.iconPlayShow = id;
      let video = document.querySelectorAll("video")[0];

      console.log("pauseVideo" + this.current);
      console.log("this.playOrPause", this.playOrPause);
      if (this.playOrPause) {
        video.pause();
        this.iconPlayShow = true;
        clearInterval(videoProcessInterval);
      } else {
        // wx.ready(() => {
        //     // 在微信的ready中进行播放 不管成功配置与否 都会执行ready
        //     video.play();
        // })
        video.play();
        video.pause();
        setTimeout(() => {
          video.play();
          this.iconPlayShow = false;
          videoProcessInterval = setInterval(() => {
            this.changeProcess(video);
          }, 100);
        }, 100);
      }
      this.playOrPause = !this.playOrPause;
      this.showShareBox = false;
    },
    //记录播放进度
    changeProcess() {
      let video = document.querySelectorAll("video")[0];
      if (video) {
        let currentTime = video.currentTime.toFixed(1);
        let duration = video.duration.toFixed(1);
        this.videoProcess = parseInt((currentTime / duration).toFixed(2) * 100);
      }
    },
  },
};
</script>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
}

.icon_play {
  position: absolute;
  top: 44%;
  right: 0;
  left: 0;
  bottom: auto;
  margin: auto;
  z-index: 999;
  height: 60px;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 50%;
}
.box {
  z-index: 999;
  position: relative;
  margin: 1px;
  width: 49.4%;
  height: calc(100vh / 3);
  float: left;
  background: #fff;
}
.mescroll {
  position: fixed;
  top: 0px;
  bottom: 15px;
  height: auto;
}
.myicon {
  z-index: 999;
  position: absolute;
  bottom: 15px;
  left: 15px;
  color: #fff;
}
</style>
