<template>
  <div class="audio-wrapper">
    <audio ref="audio">
      <source :src="myAudioUrl" type="audio/mp3" />
    </audio>
    <div class="audio-left">
      <img
        ref="audioPlayer"
        v-if="myPlayTemp"
        @click="myPlay()"
        src="https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/audio.png"
      />
      <img
        ref="audioPlayer"
        v-if="!myPlayTemp"
        @click="myPlay()"
        src="https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/play.gif"
        class="play-img"
      />
    </div>
    <div class="audio-right">
      <!-- <p style="max-width: 536px;"></p> -->
      <div class="audio-time">
        <span class="audio-length-current" ref="audioCurTime">00:00</span>
        <div class="progress-bar-bg" ref="progressBarBg" @mousedown="handledown()">
          <span ref="progressDot"></span>
          <div class="progress-bar" ref="progressBar"></div>
        </div>
        <span class="audio-length-total" ref="duration">00:00</span>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "myAudio",
  props: ["myAudioUrl"],
  data() {
    return {
      audio: "",
      myPlayTemp: true,
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.audio = this.$refs.audio;
      this.audio.load();
      this.audio.pause();
      this.updateProgress();
    },
    // 点击播放/暂停图片时，控制音乐的播放与暂停
    myPlay() {
      if (this.audio.paused) {
        // 开始播放当前点击的音频
        this.audio.play();
        this.myPlayTemp = false;
      } else {
        this.audio.pause();
        this.myPlayTemp = true;
      }
      this.updateProgress();
    },
    // 更新进度条与当前播放时间
    updateProgress() {
      let value = this.audio.currentTime / this.audio.duration;
      this.$refs.progressBar.style.width = value * 100 + "%";
      this.$refs.progressDot.style.left = value * 100 + "%";
      // 初始时间
      this.$refs.audioCurTime.innerHTML = this.transTime(
        this.audio.currentTime
      );
      // 总共时长
      let audioElement = new Audio(this.myAudioUrl);
      let self = this;
      audioElement.addEventListener("loadedmetadata", function() {
        let duration2 = (audioElement.duration / 100)
          .toFixed(2)
          .replace(".", ":");
        self.$refs.duration.innerHTML = self.formatTime(duration2);
      });
    },
    // 播放结束时
    audioEnded() {
      this.$refs.progressBar.style.width = 0 + "%";
      this.$refs.progressDot.style.left = 0 + "%";
      this.myPlayTemp = true;
    },
    // 播放时间换算
    transTime(value) {
      let time = "";
      let h = parseInt(value / 3600);
      value %= 3600;
      let m = parseInt(value / 60);
      let s = parseInt(value % 60);
      if (h > 0) {
        time = this.formatTime(h + ":" + m + ":" + s);
      } else {
        time = this.formatTime(m + ":" + s);
      }
      return time;
    },
    //  * 格式化时间显示，补零对齐
    //  * eg：2:4  -->  02:04
    formatTime(value) {
      let time = "";
      let s = value.split(":");
      let i = 0;
      for (; i < s.length - 1; i++) {
        time += s[i].length == 1 ? "0" + s[i] : s[i];
        time += ":";
      }
      time += s[i].length == 1 ? "0" + s[i] : s[i];
      return time;
    },
    // 点击进度条跳到指定点播放
    handledown() {
      // 只有音乐开始播放后才可以调节，已经播放过但暂停了的也可以
      console.log(this.audio.currentTime);
      if (!this.audio.paused || this.audio.currentTime != 0) {
        let pgsWidth = this.$refs.progressBarBg.offsetWidth;
        let rate = event.offsetX / pgsWidth;
        this.audio.currentTime = this.audio.duration * rate;
        this.updateProgress();
      }
    }
  },
  watch: {
    audio: function(params) {
      let self = this;
      self.audio.addEventListener(
        "timeupdate",
        function() {
          // console.log('开始');
          self.updateProgress();
        },
        false
      );
      self.audio.addEventListener(
        "ended",
        function() {
          // console.log('结束');
          self.audioEnded();
        },
        false
      );
    }
  }
};
</script>
 
<style lang="less" scoped>
.audio-wrapper {
 
  // background-color: #f8f8f8;
  // margin: 10px auto;
  width: 640px;
  height: 58px;
  color: #fff;
  font-size: 16px;
  // border-radius: 50px;
}

.audio-left {
  float: left;
  text-align: center;
  width: 58px;
  height: 100%;
  background-color: rgba(255,255,255,.3);
  border-radius: 50%;
}

.audio-left img {
  width: 23px;
  height: 28px;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  margin: 0;
  cursor: pointer;
}
.audio-left .play-img{
  width: 100%;
  height: 100%;
}
.audio-right {
  float: left;
  width: 555px;
  margin-left: 20px;
  height: 100%;
}

.progress-bar-bg {
  background-color: #d9d9d9;
  position: relative;
  height: 2px;
  cursor: pointer;
}

.progress-bar {
  background-color: #649fec;
  width: 0;
  height: 2px;
}

.progress-bar-bg span {
  content: " ";
  width: 10px;
  height: 10px;
  border-radius: 50%;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  background-color: #3e87e8;
  position: absolute;
  left: 0;
  top: 50%;
  margin-top: -5px;
  margin-left: -5px;
  cursor: pointer;
}

.audio-time {
  margin-top: 5%;
}

.audio-length-total {
  float: right;
  margin-top: 13px;
}

.audio-length-current {
  float: left;
  margin-top: 13px;
}
</style>
