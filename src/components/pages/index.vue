<template>
    <div>
        <div class="footerImgBox">
          <div class="footerImg"></div>
        </div>
        <div class="footerImgBox">
          <div class="footerImg2"></div>
        </div>
        <div class="cloudBox">
          <div class="cloud1"></div>
        </div>
        <div class="cloudBox">
          <div class="cloud2"></div>
        </div>
        <div class="cloudBox">
          <div class="cloud3"></div>
        </div>
        <div class="righterImgBox" v-if="false">
          <img src="../../../static/img/righter.png" class="righterImg">
        </div>
        <div class="carouselBox">
            <!--<img :src="bgImgUrl[bgImgIndex].imageUrl" class="carousel run-animation" id='carousel'>-->
          <div :style="{'background': bgImgUrl.length === 0?'#141422 top/cover fixed':'url('+bgImgUrl[bgImgIndex].imageUrl+') top/cover fixed'}" id='carousel' class="carousel run-animation"></div>
        </div>
        <div class="base6"></div>
        <div class="welcome">
            <div class="textBox" @mouseover="startFn" @mouseout="endFn">
                <h1 :data-start="start" :data-startB="startB" style="font-weight: bold;" class="text" :data-text="welcome">{{welcome}}</h1>
            </div>
            <div class="sysMsgBox" style="cursor:pointer">
                <marquee class="sysMsg" @click="jmpAnnounce">{{announceInfo?announceInfo.title:'暂无公告'}}</marquee>
                <p @click="jmpOnlineList">在线观看人数: {{online?online:0}}<el-badge value="hot" class="onlineBadge"></el-badge></p>
            </div>
            <div class="barrageBox" v-if="false">
                <input class="barrageInput" placeholder="按ESC返回" style="width:140px; padding-left:10px;padding-right:10px;">
                <p class="sendBtn">点击发送</p>
            </div>
        </div>
        <!--<div class="hotImgBox">-->
            <!--<div class="hotImgInBox">-->
                <!--<div class="hotImgNow" @click="jmpNowBangumi" :style="{'margin-left':hotImgNowLoc,width:imgWidth}"></div>-->
                <!--<img v-for="(url,index) in bgImgUrl" :src="url.imageUrl" :style="[{width:imgWidth}]" @click="jmpToBangumi(url)" class="hotImgItemBox" @mouseover="userChangeHotImgNow(index)" :key="url.bangumiId">-->
            <!--</div>-->
        <!--</div>-->
      <div class="hotImgBox">
        <div class="hotImgInBox">
          <div class="hotImgNow" @click="jmpNowBangumi" @mouseover="showImgTitle=true" @mouseout="showImgTitle=false" :style="{'margin-left':hotImgNowLoc,width:imgWidth}"></div>
          <div v-for="(url,index) in bgImgUrl" :style="[{width:imgWidth}]" @click="jmpToBangumi(url)" class="hotImgItemBox" @mouseover="userChangeHotImgNow(index)" :key="url.id">
            <el-tooltip :visible-arrow=false :content="url.title" placement="bottom" effect="dark" :manual=false :value="(showImgTitle&&bgImgIndex==index)||bgImgIndex==index">
              <img :src="url.imageUrl" style="height: 96px;width:100%">
            </el-tooltip>
          </div>
        </div>
      </div>
      <div class="indexFooter">
        <div class="inderFooter2">
          <div class="footerAbout">
            <a target="_blank" class="footerItem" href="#/user/1">bug反馈</a>
            <a target="_blank" class="footerItem" href="#/aboutUs">关于我们</a>
            <a target="_blank" class="footerItem" href="#/contactUs" @mouseover="infoText='联系我们'" @mouseout="infoText='用爱发电'">{{infoText}}</a>
            <a target="_blank" class="footerItem" href="https://t.me/joinchat/D6RlYg6geGuiS3WP_ag5zg"><img style="width:20px;height:15px;" src="../../../static/img/logo.png"><p>加入我们</p></a>
          </div>
          <div class="authorInfo">
            <p>Crafted with</p>
            <p style="color:red;font-size:18px;">❤</p>
            <p>by Darkers ©2018 Darker.</p>
          </div>
          <div class="indexFooterInBox">
            <a target="_blank" :href="GLOBAL.DOInviteLink" class="footerItem"><img class="elementIcon" :src="GLOBAL.imgURL+'/FlgYbSr.png'"></a>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import api from "../../api.js";
export default {
  data() {
    return {
      start: false,
      startB: false,
      timer: null,
      carouselTimer: null,
      bgImgUrl: [{imageUrl:'../../../static/img/1.jpg'}],
      bgImgIndex: 0,
      hotImgNowLoc: "0px",
      announceInfo: "",
      imgWidth:'',
      widthNum:0,
      infoText:'用爱发电',
      welcome:'Welcome to darker!!',
      online:'',
      showImgTitle: false
    };
  },
  methods: {
    startFn: function() {
      this.start = true;
      var that = this;
      this.timer = setTimeout(function() {
        that.startB = true;
      }, 2000);
    },
    endFn: function() {
      this.start = false;
      clearTimeout(this.timer);
      this.startB = false;
    },
    userChangeHotImgNow: function(index) {
      clearInterval(this.carouselTimer);
      this.carouselTimer=null;
      this.bgImgIndex = index;
      this.hotImgNowLoc = (this.widthNum * this.bgImgIndex).toString() + "%";
      let carousel = document.getElementById("carousel");
      carousel.classList.remove("run-animation");
      carousel.offsetLeft;
      carousel.classList.add("run-animation");
      this.carouselTimer = setInterval(()=>{
        this.bgImgIndex++;
        if (this.bgImgIndex == this.bgImgUrl.length) this.bgImgIndex = 0;
        this.hotImgNowLoc = (this.widthNum * this.bgImgIndex).toString() + "%";
      }, 5000);
    },
    jmpToBangumi(item){
      console.log(item);
      window.location.assign(item.linkUrl);
    },
    jmpNowBangumi(){
      if(this.bgImgUrl[this.bgImgIndex]){
        window.location.assign(this.bgImgUrl[this.bgImgIndex].linkUrl);
      }
    },
    jmpAnnounce() {
      if(this.announceInfo){
        this.$router.push({ name: "announce" ,params:{id:this.announceInfo.id} });
      }
    },
    jmpOnlineList(){
      this.$router.push({ name: "onlineList"});
    },
    async initAnnounce(){
      let resData=(await api.getNotice()).data;
      if(resData.code===0){
        this.announceInfo=resData.data.indexNotice;
        console.log('公告信息',this.announceInfo);
      }
    },
    async initIndexInfo(){
      let res=(await api.getIndex()).data;
      console.log('初始化首页新接口',res);
      if(res.code==0){
        this.online=res.data.online_watch_count;
        (res.data.index_sentence!=''||!res.data.index_sentence)?this.welcome=res.data.index_sentence:this.welcome;
        this.bgImgUrl=res.data.index_recommend;
        if(this.bgImgUrl.length === 0){
          this.imgWidth = "100%";
          this.widthNum = 0;
        }
        else {
          this.imgWidth=(100/this.bgImgUrl.length).toString()+'%';
          this.widthNum=100/this.bgImgUrl.length;
        }
      }
    }
  },
  created() {
    //在这里获取首页推荐的数据
    this.initIndexInfo().then(()=>{
      this.carouselTimer = setInterval(()=> {
        this.bgImgIndex++;
        if (this.bgImgIndex == this.bgImgUrl.length) this.bgImgIndex = 0;
        this.hotImgNowLoc = (this.widthNum * this.bgImgIndex).toString() + "%";
      }, 5000);
    });
    this.initAnnounce();
    this.$emit('toIndex');
    console.log("index created!!!");
  }
};
</script>

<style scoped>
.indexFooter {
  position: absolute;
  bottom: 0;
  z-index: 1000;
  display: flex;
  flex-direction: row;
  left: 0.5%;
  width: 99%;
}
.inderFooter2 {
  display: flex;
  flex-direction: row;
  margin: auto auto;
}
.footerAbout {
  display: flex;
  flex-direction: row;
}
.authorInfo {
  display: flex;
  flex-direction: row;
  height: 20px;
}
.authorInfo p {
  height: 20px;
  margin: auto 3px;
  line-height: 20px;
}
.indexFooterInBox {
  display: flex;
  flex-direction: row;
  margin: auto auto;
}
.footerItem {
  margin: auto 10px;
  text-align: center;
  display: flex;
  flex-direction: row;
  height: 20px;
  text-decoration: none;
  color: black;
}
.footerItem img {
  height: 20px;
  width: 20px;
  margin: auto auto;
}
.elementIcon {
  height: 20px;
  width: 120px !important;
}
.footerItem p {
  line-height: 20px;
  height: 20px;
  margin: auto 5px;
}
.carouselBox {
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: absolute;
  z-index: 1;
}
.carousel {
  width: 100%;
  height: 100%;
  margin: 0 0;
  /*滤镜*/
  filter: grayscale(30%);
  background-size: cover;
  background-repeat: no-repeat;
}
.run-animation {
  animation: carouselMove 5s linear 0s infinite normal;
}
@keyframes carouselMove {
  100% {
    transform: scale(1.1, 1.1);
  }
}
.base6 {
  display: block;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-image: url(https://cdn.jsdelivr.net/gh/moezx/cdn@3.1.9/img/Sakura/images/dot.gif);
  z-index: 4;
}
.welcome {
  position: absolute;
  height: 85px;
  width: 50%;
  display: flex;
  flex-direction: column;
  z-index: 999;
  top: 340px;
  left: 25.5%;
  /*border-left: 10px solid black;*/
  /*border-right: 10px solid black;*/
}
.textBox {
  position: absolute;
  height: 90%;
  width: 100%;
  top: 20%;
  margin: auto auto;
}
.text {
  /*background: rgb(228, 166, 8);*/
  background: rgb(42, 83, 103);
  opacity: 0.8;
  position: absolute;
  height: 90px;
  width: 100%;
  font-size: 50px;
  line-height: 90px;
  top: -31px;
  color: rgb(0, 0, 0);
  clip: rect(0px, 1000px, 80px, 0px);
  font-weight: bold;
}
[data-start="true"] {
  animation: textShiningA1 2s linear 0s 1 normal;
}
[data-startB="true"] {
  animation: textShiningA2 4s linear 2s infinite normal;
}
[data-start="true"].text:before,
[data-start="true"].text:after {
  content: attr(data-text);
  position: absolute;
  width: 100%;
  background: rgba(0, 0, 0, 0);
  clip: rect(0px, 1000px, 80px, 0px);
  opacity: 0.6;
  font-weight: bold;
}
[data-start="true"].text:before {
  top: -1px;
  left: 3px;
  text-shadow: 2px 2px #35ff1a;
  animation: textShiningB1 1.5s linear 2.5s infinite normal;
}
[data-start="true"].text:after {
  top: 1px;
  left: -3px;
  text-shadow: -2px -2px #00a7e0;
  animation: textShiningB2 1.5s linear 2.5s infinite normal;
}
@keyframes textShiningA1 {
  0% {
    clip: rect(15px, 1000px, 25px, 0px);
    opacity: 1;
  }
  70% {
    clip: rect(60px, 1000px, 70px, 0px);
    opacity: 1;
  }
  71% {
    opacity: 0;
  }
  77% {
    clip: rect(0px, 1000px, 85px, 0px);
    opacity: 1;
  }
  85% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes textShiningA2 {
  30% {
    opacity: 1;
    left: 4px;
    color: black;
  }
  35% {
    opacity: 1;
    left: 4px;
    color: gray;
  }
  37% {
    opacity: 0;
    color: black;
  }
  39% {
    opacity: 1;
    left: 0px;
  }
}
@keyframes textShiningB1 {
  0% {
    clip: rect(0, 0, 0, 0px);
    opacity: 1;
  }
  14% {
    clip: rect(10px, 1000px, 20px, 0px);
  }
  15% {
    clip: rect(10px, 1000px, 20px, 0px);
  }
  35% {
    clip: rect(25.75px, 1000px, 35.75px, 0px);
  }
  37% {
    clip: rect(0px, 1000px, 80px, 0px);
  }
  39% {
    clip: rect(27.55px, 1000px, 37.55px, 0px);
  }
  65% {
    clip: rect(35.75px, 1000px, 42.25px, 0px);
    text-shadow: 2px 2px #35ff1a;
  }
  67% {
    clip: rect(27.85px, 1000px, 43.55px);
    text-shadow: 80px 80px #35ff1a;
  }
  69% {
    clip: rect(37.95px, 1000px, 44.85px, 0px);
    text-shadow: 2px 2px #35ff1a;
  }
  100% {
    clip: rect(55px, 1000px, 65px, 0px);
    opacity: 1;
  }
}
@keyframes textShiningB2 {
  0% {
    clip: rect(0, 0, 0, 0px);
    opacity: 1;
  }
  14% {
    clip: rect(12px, 1000px, 25px, 0px);
  }
  15% {
    clip: rect(12px, 1000px, 25px, 0px);
  }
  35% {
    clip: rect(25.75px, 1000px, 35.75px, 0px);
  }
  37% {
    clip: rect(0px, 1000px, 80px, 0px);
  }
  39% {
    clip: rect(27.55px, 1000px, 37.55px, 0px);
  }
  65% {
    clip: rect(35.75px, 1000px, 42.25px, 0px);
    text-shadow: -2px -2px #00a7e0;
  }
  67% {
    clip: rect(36.85px, 1000px, 52.55px);
    text-shadow: -80px -80px #00a7e0;
  }
  69% {
    clip: rect(37.95px, 1000px, 44.85px, 0px);
    text-shadow: -2px -2px #00a7e0;
  }
  100% {
    clip: rect(57px, 1000px, 70px, 0px);
    opacity: 1;
  }
}
.sysMsgBox {
  position: absolute;
  top: -13px;
  /*background: rgb(228, 166, 8);*/
  background: rgb(3, 4, 4);
  opacity: 0.9;
  height: 30px;
  width: 100%;
  border-bottom: 3px solid gray;
  display: flex;
  flex-direction: row;
}
.sysMsgBox p{
  color: wheat;
  margin: auto auto;
  text-align: center;
  width: 30%;
  height: 30px;
  line-height: 30px;
  font-size: 15px;
  font-weight: bold;
  border-left: 2px solid grey;
  position: relative;
}
.onlineBadge{
  margin: 5px 5px;
  position: absolute;
}
.sysMsg {
  line-height: 30px;
  color: #c19e33;
  width: 80%;
  font-weight: bold;
  margin: auto auto;
}
.hotImgBox {
  background: transparent;
  border-top: 3px solid #7a7676;
  border-radius: 0px;
  position: absolute;
  height: 10%;
  width: 50%;
  /*绝对布局的居中法*/
  top: 440px;
  left: 25.4%;
  display: flex;
  flex-direction: row;
  z-index: 1000;
}
.hotImgInBox {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: row;
  margin: auto auto;
}
.hotImgItemBox {
  cursor: pointer;
}
.hotImgInBox img {
  height: 100%;
  width: inherit;
  /*
    置于a底下
    position: relative;
    z-index: -1;
    */
}
.hotImgNow {
  position: absolute;
  height: 96px;
  border: 1px solid white;
  box-shadow: 0px 0px 30px white inset;
  cursor: pointer;
  margin-top: 0px;
  transition: margin-left 1s;
}
.barrageBox {
  background: grey;
  opacity: 0.8;
  position: absolute;
  height: 30px;
  width: 300px;
  display: flex;
  flex-direction: row;
  /*绝对布局的居中法*/
  top: 50%;
  left: 50%;
  transform: translate(-50%, 700%);
}
.barrageInput,
.sendBtn {
  margin: auto auto;
  line-height: 30px;
}
.barrageInput {
  width: 100px;
  height: 20px;
}
.sendBtn {
  width: 100px;
  height: 30px;
}
.footerImgBox {
  position: absolute;
  z-index: 3;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.footerImg {
  margin: auto auto;
  margin-bottom: 0;
  width: 100%;
  height: 65px;
  background-image: url("../../../static/img/wave1.png");
  background-position: 0 0;
  animation: footerMv 10s infinite linear;
}
@keyframes footerMv {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 130% 0;
  }
}
.footerImg2 {
  margin: auto auto;
  margin-bottom: 0;
  width: 100%;
  height: 70px;
  background-image: url("../../../static/img/wave2.png");
  animation: footerMv 20s infinite 0.2s linear;
  display: flex;
  flex-direction: column;
}
.footerImg2 p {
  margin: auto auto;
  margin-bottom: 0;
  cursor: pointer;
}
.righterImgBox {
  position: absolute;
  z-index: 2;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.righterImg {
  margin: auto auto;
  margin-right: 0;
  width: 100px;
}
.cloudBox {
  position: absolute;
  z-index: 3;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.cloud1 {
  margin: auto auto;
  margin-right: -180px;
  margin-bottom: 180px;
  width: 180px;
  height: 90px;
  background-image: url("../../../static/img/cloud1.png");
  background-size: cover;
  animation: cloudMv 20s infinite linear;
}
@keyframes cloudMv {
  0% {
    margin-right: -180px;
  }
  99% {
    margin-right: 1600px;
    opacity: 0;
  }
  100% {
    opacity: 0;
  }
}
.cloud2 {
  margin: auto auto;
  margin-right: -180px;
  margin-bottom: 650px;
  width: 180px;
  height: 86.4px;
  background-image: url("../../../static/img/cloud2.png");
  background-size: cover;
  animation: cloudMv 25s infinite 7s linear;
}
.cloud3 {
  margin: auto auto;
  margin-right: -100px;
  margin-bottom: 550px;
  width: 100px;
  height: 70px;
  background-image: url("../../../static/img/cloud3.png");
  background-size: cover;
  animation: cloudMv2 30s infinite 15s linear;
}
@keyframes cloudMv2 {
  0% {
    margin-right: -100px;
  }
  99% {
    margin-right: 1600px;
    opacity: 0;
  }
  100% {
    opacity: 0;
  }
}
</style>
