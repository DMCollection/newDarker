<template>
    <div class="msgBox">
        <div class="msgInbox" @mouseover="show=true" @mouseout="show=false;">
            <menu-item-button :class="['msgTitle',{'animation-run':unreadNoticeNumTmp.total>0}]" icon-img="../../../static/img/notice.png">消息<el-badge v-if="unreadNoticeNumTmp.total" :value="unreadNoticeNumTmp.total" class="unreadNum" :max="999"></el-badge></menu-item-button>
            <transition name="msgMenu">
                <div v-if="show" class="msgMenuBox">
                    <div class="msgItemBox">
                        <p class="msgItem" @mouseover="selecting1=true" @click="jmpNotice(0)">
                          系统通知
                          <el-badge :value="unreadNoticeNumTmp.system" v-if="unreadNoticeNumTmp.system>0" :style="{'margin-top':'auto'}" :max="999"></el-badge>
                        </p>
                        <p class="msgItem" @click="jmpNotice(1)" @mouseover="selecting2=true">回复我的 <el-badge :value="unreadNoticeNumTmp.reply" v-if="unreadNoticeNumTmp.reply>0" :style="{'margin-top':'auto'}" :max="999"></el-badge></p>
                        <p class="msgItem" @click="jmpNotice(2)" @mouseover="selecting3=true">@我的 <el-badge :value="unreadNoticeNumTmp.at" v-if="unreadNoticeNumTmp.at>0" :style="{'margin-top':'auto'}" :max="999"></el-badge></p>
                        <p class="msgItem" @click="jmpNotice(3)" @mouseover="selecting4=true">收到的赞 <el-badge :value="unreadNoticeNumTmp.like" v-if="unreadNoticeNumTmp.like>0" :style="{'margin-top':'auto'}" :max="999"></el-badge></p>
                    </div>
                </div>
            </transition>
            <transition name="selectingTran">
              <span class="selecting selecting1" @mouseout="selecting1=false" v-show="selecting1" @click="jmpNotice(0)">
                系统通知
                <el-badge :value="unreadNoticeNumTmp.system" v-if="unreadNoticeNumTmp.system>0" :style="{'margin-top':'auto'}" :max="999"></el-badge>
              </span>

            </transition>
            <transition name="selectingTran">
              <span class="selecting selecting2" @mouseout="selecting2=false" v-show="selecting2" @click="jmpNotice(1)">
                回复我的
                <el-badge :value="unreadNoticeNumTmp.reply" v-if="unreadNoticeNumTmp.reply>0" :style="{'margin-top':'auto'}" :max="999"></el-badge>
              </span>
            </transition>
            <transition name="selectingTran">
              <span class="selecting selecting3" @mouseout="selecting3=false" v-show="selecting3" @click="jmpNotice(2)">
                @我的
                <el-badge :value="unreadNoticeNumTmp.at" v-if="unreadNoticeNumTmp.at>0" :style="{'margin-top':'auto'}" :max="999"></el-badge>
              </span>
            </transition>
            <transition name="selectingTran">
              <span class="selecting selecting4" @mouseout="selecting4=false" v-show="selecting4" @click="jmpNotice(3)">
                收到的赞
                <el-badge :value="unreadNoticeNumTmp.like" v-if="unreadNoticeNumTmp.like>0" :style="{'margin-top':'auto'}" :max="999"></el-badge>
              </span>
            </transition>
        </div>
    </div>
</template>

<script>
import MenuItemButton from "./MenuItemButton.vue";
export default {
  props: ["unreadNoticeNum"],
  components: {
    MenuItemButton
  },
  data() {
    return {
      show: false,
      unreadNoticeNumTmp: this.unreadNoticeNum,
      selecting1: false,
      selecting2: false,
      selecting3: false,
      selecting4: false
    };
  },
  watch: {
    unreadNoticeNum() {
      console.log("未读", this.unreadNoticeNum);
      this.unreadNoticeNumTmp = this.unreadNoticeNum;
    },
    show() {
      if (!this.show) {
        this.selecting1 = false;
        this.selecting2 = false;
        this.selecting3 = false;
        this.selecting4 = false;
      }
    }
  },
  methods: {
    jmpNotice(pageNum) {
      this.$emit("jmpNotice", pageNum);
    }
  }
};
</script>

<style scoped>
.selecting {
  background: rgb(255, 255, 255);
  position: absolute;
  right: -97%;
  overflow: hidden;
  height: 45px;
  width: 130px;
  display: flex;
  flex-direction: column;
  line-height: 45px;
  cursor: pointer;
  font-size: 15px;
  opacity: 1;
}
.selecting1 {
  top: 65px;
}
.selecting2 {
  top: 100px;
}
.selecting3 {
  top: 135px;
}
.selecting4 {
  top: 170px;
}
.selectingTran-leave-active,
.selectingTran-enter-active {
  transition: all 0.1s ease;
}
.selectingTran-leave-active,
.selectingTran-enter {
  right: -90% !important;
  width: 120px !important;
  font-size: 13px !important;
  opacity: 0 !important;
  /*!important 将该样式优先级调至最高*/
}
.selectingTran-leave,
.selectingTran-enter-active {
  width: 130px;
  right: -97%;
  font-size: 15px;
  opacity: 1;
}
.msgBox {
  width: 60px;
  height: 60px;
  display: flex;
  flex-direction: column;
}
.msgInbox {
  width: 60px;
  height: 60px;
  display: flex;
  flex-direction: column;
  position: relative;
}
.msgTitle {
  cursor: pointer;
}
.msgMenu-leave-active,
.msgMenu-enter-active {
  transition: all 0.5s ease;
}
.msgMenu-leave-active,
.msgMenu-enter {
  height: 0px !important;
  /*!important将该样式优先级调至最高*/
  opacity: 0;
}
.msgMenu-leave,
.msgMenu-enter-active {
  height: 100px;
}
.msgMenuBox {
  background: rgba(255, 255, 255, 0.8);
  position: absolute;
  right: -90%;
  top: 60px;
  overflow: hidden;
  height: 160px;
  width: 120px;
  border-radius: 3px;
  display: flex;
  flex-direction: column;
}
.msgItemBox {
  width: 100%;
  height: 140px;
  margin: auto;
  display: flex;
  flex-direction: column;
  font-size: 13px;
}
.msgItem {
  height: 35px;
  width: 100%;
  line-height: 35px;
  margin: 0;
  cursor: pointer;
  transition: background 0.4s;
}
.msgItem:hover {
  background: rgba(100, 149, 237, 0.6);
}
.unreadNum {
  position: absolute;
  top: 12px;
}
.unreadNum p {
  margin: 0 0;
  color: white;
  font-size: 10px;
  line-height: 15px;
}
.unReadItemNum {
  color: red;
  font-size: 12px;
  font-family: 华文琥珀,serif;
}
.animation-run {
  background: rgba(255, 166, 0, 0.5);

  animation: shining 2s infinite;
}
@keyframes shining {
  0% {
    filter: grayscale(0%);
    background: rgba(255, 166, 0, 0.5);
    box-shadow: 0px 0px 15px rgba(255, 166, 0, 0.5);
  }
  50% {
    filter: grayscale(100%);
    background: rgba(255, 166, 0, 0);
    box-shadow: 0px 0px 0px rgba(255, 166, 0, 0);
  }
  100% {
    filter: grayscale(0%);
    background: rgba(255, 166, 0, 0.5);
    box-shadow: 0px 0px 15px rgba(255, 166, 0, 0.5);
  }
}
</style>
