<template>
  <div class="home">
    <video id="video" autoplay width="600" height="400"></video>
    <video id="video1" autoplay width="600" height="400"></video>
    <button @click="movePosition">点击移动webStreamer的位置</button>
    <button @click="webRtcServer1.disconnect()">关闭stream</button>
    <button
      @click="
        webRtcServer1.connect(
          'rtsp://admin:admin123@192.168.13.66:554/Streaming/Channels/101?transportmode=unicast&profile=Profile_1'
        )
      "
    >
      开启stream
    </button>
  </div>
</template>

<script>
import { handleError } from "vue";

// import "../../public/static/webrtcstreamer"
// import "../../public/static/adapter.min.js"
export default {
  name: "HomeView",
  data() {
    return {
      webRtcServer: null,
      webRtcServer1: null,
      iceServer: null,
      webStreamerContainer: [],
      deviceMapWebStreamer: new WeakMap(),
      deviceList: [{ id: 1 }],
    };
  },
  async mounted() {
    //192.168.1.100是启动webrtc-streamer的服务器IP，8000是webrtc-streamer的默认端口号，可以在服务端启动的时候更改的
    // this.webRtcServer = new WebRtcStreamer(
    //   "video",
    //   location.protocol + "//192.168.10.110:8000"
    // );
    //需要看的rtsp视频地址，可以在网上找在线的rtsp视频地址来进行demo实验，在vlc中能播放就能用
    // this.webRtcServer.connect(
    //   "rtsp://wowzaec2demo.streamlock.net/vod/mp4:BigBuckBunny_115k.mp4"
    // );
    // this.webRtcServer.connect(
    //   "rtsp://admin:admin123@192.168.13.66:554/Streaming/Channels/101?transportmode=unicast&profile=Profile_1"
    // );

    this.createIceServer();
    this.webRtcServer1 = this.createWebStreamer("video1");
    this.webRtcServer1.connect();

    this.webStreamerContainer.push(this.webRtcServer1);
    this.deviceMapWebStreamer.set(this.deviceList[0], this.webRtcServer1);
  },
  watch: {
    deviceList: {
      handler(val) {
        console.log("***", val);
        val.forEach((item) => {
          if (!this.deviceMapWebStreamer.get(item)) {
            // TODOS: 创建 webStreamer 流
            // ...

            // 再添加到 WeakMap 中
            this.deviceMapWebStreamer.set(
              item,
              this.webRtcServer1 /** 此处应该是新建的 streamer 对象 */
            );
          }
        });
      },
      deep: true,
    },
  },
  methods: {
    // 调试方法
    movePosition() {
      this.deviceList.unshift({ id: "12" });
      console.log(this.deviceList, this.deviceMapWebStreamer);
    },
    // 创建iceServer
    async createIceServer() {
      const res = await fetch(
        "http://192.168.10.110:8000" + "/api/getIceServers"
      );
      const response = await res.json();
      return response;
    },
    // 返回iceServer 的单例对象
    getIceServer() {
      if (this.iceServer) {
        return this.iceServer;
      } else {
        this.iceServer = this.createIceServer();
      }
    },
    /**
     * 创建 webStreamer 对象
     * @param {*} elementId dom元素的id或者dom本身
     */
    createWebStreamer(elementId) {
      const webStreamer = new WebRtcStreamer(elementId);
      webStreamer.iceServers = this.getIceServer();
      webStreamer.srvurl = "http://192.168.10.110:8000";
      return webStreamer;
    },
  },
  beforeDestroy() {
    // this.webRtcServer.disconnect();
    // this.webRtcServer = null;
    this.webRtcServer1.disconnect();
    this.webRtcServer1 = null;
  },
};
</script>

<style lang="less">
.offline {
  position: relative;
  &::after {
    position: absolute;
    width: 100%;
    height: 100%;
    text-align: center;
    line-height: 100%;
    left: 0;
    top: 0;
    background-color: rgba(0, 0, 0, 0.1);
    content: "设备已断开";
    font-size: 24px;
    color: #fff;
  }
}
</style>
