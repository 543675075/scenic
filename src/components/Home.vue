<template>
  <div class="home" ref="home">
    <div class="bg-box"></div>
    <div class="bg-color"></div>
    <div class="title">{{obj.name}}</div>
    <div class="outer">
      <div class="container">
        <scroll-better class="scroll">
          <div class="desc desc1">
            <span>{{obj.desc1}}</span>
          </div>
          <img :src="obj.img" class="show-img" />
          <div class="desc desc2">
            <span>{{obj.desc2}}</span>
          </div>
        </scroll-better>
      </div>

      <img class="shadow shadow-t" src="https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/bottomshadow1.png" />
      <img class="shadow shadow-b" src="https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/bottomshadow2.png" />
    </div>
    <my-audio ref="myAudioOne" :myAudioUrl="obj.audio" class="my-audio"></my-audio>
  </div>
</template>



<script>
import ScrollBetter from "./scrollBetter";
import myAudio from "./Audio";
export default {
  name: "Home",
  components: {
    ScrollBetter,
    myAudio
  },
  data() {
    return {
      obj: {
        name: "黄山湖公园",
        // img: require("../assets/show.png"),
        img: 'https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/show.png',
        audio: "http://mp.111ttt.cn/mp3music/97247885.mp3",
        desc1:
          "黄山湖是我国AAAA级公园，公园建设将充分利用大桥、山、长江、湖等现有的旅游资源，以市场意识为导向，建设性、保护性、可持续发展为基准点，建成一个符合现代游憩功能需求、生态俱全、景观优美、延续地方文脉、发展城市风采、实现良性生态循环的新型生态",
        desc2:
          "其中黄山湖作为公园主题，面积几近整个区域的一半，在中央竖琴广场和湖之间设计了一系列的水上游乐设施，包括游艇、皮划艇、其中黄山湖作为公园主题，面积几近整个区域的一半，在中央竖琴广场和湖之间设计了一系列的水上游乐设施，包括游艇、皮划艇其中黄山湖作为公园主题，面积几近整个区域的一半，在中央竖琴广场和湖之间设计了一系列的水上游乐设施，包括游艇、皮划艇其中黄山湖作为公园主题，面积几近整个区域的一半，在中央竖琴广场和湖之间设计了一系列的水上游乐设施，包括游艇、皮划艇其中黄山湖作为公园主题，面积几近整个区域的一半，在中央竖琴广场和湖之间设计了一系列的水上游乐设施，包括游艇、皮划艇"
      }
    };
  },
  mounted() {
    // const h =
    //   window.innerHeight ||
    //   document.documentElement.clientHeight ||
    //   document.body.clientHeight;
    // console.log(h);
    // this.$refs.home.style.height = h + "px";
  },
  watch: {
    dialogVisible: function(val) {
      const self = this;
      self.$refs.myAudioOne.init();
      self.$refs.myAudioOne.audioEnded();
    }
  }
};
</script>
<style lang='less' scoped>
.home {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: url("https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/bg.png") no-repeat center/cover;
  .bg-box {
    width: 100%;
    height: 100%;
    background: url("https://cyl-blog-1300455867.cos.ap-shanghai.myqcloud.com/travel/assets/bg.png") no-repeat center/cover;
    filter: blur(10px);
  }
  .bg-color {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 430px;
    background-color: #4da3b5;
  }
  .title {
    position: absolute;
    left: 67px;
    top: 108px;
    color: #fff;
    font: bold 60px "思源黑体 CN";
    &::before {
      position: absolute;
      left: 0;
      top: -31px;
      content: "";
      width: 150px;
      height: 8px;
      background-color: #fff;
    }
  }
  .outer {
    position: absolute;
    top: 211px;
    left: 50%;
    width: 660px;
    height: calc(100% - 211px - 160px);
    transform: translateX(-50%);
    line-height: 0 !important;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .container {
    width: 100%;
    height: 100%;
    background-image: linear-gradient(
      to bottom,
      rgba(255, 255, 255, 1) 50%,
      rgba(255, 255, 255, 1) 50%,
      rgba(255, 255, 255, 0.8) 100%
    );
    border-radius: 10px;
    padding: 59px 49px;
    box-sizing: border-box;
    overflow: hidden;
  }
  .scroll {
    height: 100%;
    overflow: hidden;
    font: 28px "思源黑体 CN";
    color: #333;
    line-height: 52px !important;
    .desc {
      text-indent: 2em;
      text-align: justify;
    }
    .show-img {
      display: block;
      margin: 42px auto 46px;
      width: 481px;
      height: 291px;
    }
  }
  .shadow {
    border-radius: 5px;
    height: 15px;
  }
  .shadow-t {
    width: 620px;
  }
  .shadow-b {
    width: 580px;
  }
  .my-audio {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 69px;
  }
}
</style>