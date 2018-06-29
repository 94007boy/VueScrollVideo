<template>
  <li :ref="content.id">
    <div class="player">
      <video-player  class="video-player vjs-custom-skin"
                     ref="videoPlayer"
                     :playsinline="true"
                     :options="playerOptions"
      >
      </video-player>
      <div class="play-btn-mask" @click="onScreenClick"/>
    </div>
  </li>
</template>
<script>
  import 'video.js/dist/video-js.css';
  import 'vue-video-player/src/custom-theme.css';
  import { videoPlayer } from 'vue-video-player';
  export default {
    name: 'MediaInnerVideo',
    props: {
      content: {
        type: Object,
        default: function () { return {};}
      }
    },
    data () {
      let that = this;
      return {
        playerOptions: {
          autoplay: false, //如果true,浏览器准备好时开始回放。
          muted: false, // 默认情况下将会消除任何音频。
          loop: false, // 导致视频一结束就重新开始。
          preload: 'auto',
          language: 'zh-CN',
          aspectRatio: '16:9', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
          fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
          isFullscreen:true,
          sources: [{
            type: "video/mp4",
            src: that.content.video //播放地址（必填）
          }],
          poster: that.content.image, //你的封面地址
          width: document.documentElement.clientWidth,
          notSupportedMessage: '此视频暂无法播放，请稍后再试', //允许覆盖Video.js无法播放媒体源时显示的默认信息。
        },
        mediaHeight:0,
        screenHeight:0,
        timeTask:null
      }
    },
    created() {
      this.screenHeight = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
      this.$bus.on('handleScroll', this.handleScroll);
    },
    components: {
      videoPlayer
    },
    methods: {
      handleScroll(top) {
        let liId = this.content.id;
        let offsetTop = this.$refs[liId].offsetTop//组件距离整个文档最顶部的高度
        let scrollTop = top.scrollTop;//文档滑过的距离
        let mediaHeight = this.$refs[liId].offsetHeight;//视频本身的高度
        let a = scrollTop - offsetTop - mediaHeight;////文档向上滑动，视频恰好滑出可见区域时
        let b = mediaHeight + this.screenHeight;
        let c = a + b;//文档向下滑动，视频恰好滑出可见区域时
        let that = this;
        if(a > 0 || c < 0){//视频不可见时，停止播放
          if(!this.player.paused()){
            this.player.pause()
          }
        }else{//视频重新可见时，开始播放,为了看到滑出暂停再播放的效果，延迟2秒。其实听声音就可以知道:-)
          this.timeTask = setTimeout(function (){
            if(that.player.paused()){
              that.player.play()
            }
          }, 2000);
        }
      },
      onScreenClick(){
        if(this.player.controls()){
          if(this.player.paused()){
            this.player.play();
          }else{
            this.player.pause();
          }
        }else{
          this.player.controls = true;
        }
      }
    },
    beforeDestroy() {
      clearTimeout(this.timeTask)
      this.$bus.off('handleScroll', this.handleScroll);
    },
    computed: {
      player() {
        return this.$refs.videoPlayer.player
      }
    }
  }
</script>

<style scoped>
  li {
    box-sizing:border-box;
    margin-top:20px;
    position:relative;
  }
  .player {
    width: 100%;
    height:calc(100vw)/16*9;
    position:relative;
    background:#000;
  }
  .play-btn-mask{
    position: absolute;
    width:100%;
    height:75%;
    top:0;
    left:0;
    z-index: 999999999999999;
  }
  video-player {
    position:absolute;
    max-height:100%;
    max-width:100%;
  }
  .bottomCtrl{
    width: 40px;
    height: 40px;
    position: absolute;
    right:0px;
    bottom:0px;
    color: #fff;
    z-index: 999999999999999;
  }
</style>
