<template>
    <div class="systemNoticeBox">
        <div class="wattingBox" v-if="loading">
          <img src='data:image/gif;base64,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'>
          <p>加载数据中...</p>
        </div>
        <p class="systemNoticeTitle">&thinsp;&thinsp; 系统通知</p>
        <div class="systemNoticeInBox">
            <darker-scroll
                :item-count=pageSize
                :item-height=80
                :item-spacing=20
                :scroll-hight=740
                :scroll-width=840
                @nextPage="nextPage"
                style="margin-top:0;"
                class="scroll">
                <div class="systemMsgItem" v-for="item in systemNoticeList" :key="item.id">
                    <div class="systemInfo">
                        <p class='systemTitle'>{{item.title}}</p>
                        <p class='systemTime'>{{new Date(item.createTime).toLocaleString()}}</p>
                    </div>
                    <p class="systemContent" v-html="item.content"></p>
                </div>
            </darker-scroll>
        </div>
    </div>
</template>

<script>
import darkerScroll from "../darkerStyleComponents/darkerScroll.vue";
import api from "../../api.js";
export default {
  components: {
    darkerScroll
  },
  data() {
    return {
      loading:false,
      systemNoticeList: [],
      pageSize: 0,
      pageNum: 1,
      totalSize: 0,
      pbParams: {
        type:4,
        pn: 1,
        ps: 20
      },
    };
  },
  methods: {
    async initMessage() {
      let rd = (await api.getMessageList({
        type: 4
      })).data;
      console.log("系统信息列表", rd);
      if (rd.code === 0) {
        this.systemNoticeList = rd.data.messages;
        this.totalSize = rd.data.page.totalSize;
        //不满或刚好一页
        if (rd.data.page.totalSize <= 20) this.pageSize = this.totalSize;
        else this.pageSize = 20;
      } else {
        console.log("请求列表失败");
      }
      this.loading = false;
    },
    async nextPage() {
      if (Math.ceil(this.totalSize / 20) == this.pageNum) return;
      this.loading=true;
      this.pbParams.pn++;
      this.pageNum++;
      let rd = (await api.getMessageList(this.pbParams)).data;
      console.log("下一页", rd);
      if (rd.code === 0) {
        this.systemNoticeList = this.systemNoticeList.concat(rd.data.messages);
        this.pageSize += rd.data.messages.length;
      } else {
        console.log("请求下一页失败");
      }
      this.loading=false;
    }
  },
  mounted() {
    this.loading=true;
    this.initMessage();
  }
};
</script>

<style scoped>
.systemNoticeBox {
  display: flex;
  flex-direction: column;
}
.systemNoticeBox a{
  text-decoration: none;
}
.systemNoticeTitle {
  margin: 10px auto;
  color: wheat;
  background: rgb(53, 50, 52);
  /*border: 1px solid rgba(255, 255, 255, 0.2);*/
  height: 40px;
  width: 91%;
  line-height: 40px;
  border-radius: 3px;
  text-align: left;
}
.systemNoticeInBox {
  margin: auto auto;
  width: 95%;
  height: 800px;
  display: flex;
  flex-direction: column;
  /*border: 1px solid rgba(255, 255, 255, 0.2);*/
  border-radius: 3px;
}
.systemMsgItem {
  /*background: rgba(32, 32, 32, 0.377);*/
  background: rgba(90, 85, 85, 0.377);
  color: white;
  border-radius: 10px;
  width: 95%;
  height: 80px;
  display: flex;
  flex-direction: column;
  margin-top: 10px;
  text-align: left;
}
.systemInfo {
  display: flex;
  flex-direction: row;
  margin-top: 10px;
}
.systemTitle {
  margin: auto 10px;
  color: #5ec3a1;
}
.systemTime {
  margin: auto 10px;
  font-size: 13px;
  color: rgb(196, 196, 196);
}
.systemContent {
  font-size: 13px;
  width: 98%;
  margin: auto 10px;
}
.wattingBox {
  background: white;
  position: absolute;
  z-index: 300;
  width: 150px;
  height: 100px;
  display: flex;
  flex-direction: column;
  border-radius: 5px;
  margin: 250px 275px;
}
.wattingBox img {
  margin: auto auto;
  margin-bottom: 0;
}
.wattingBox p {
  margin: auto auto;
  font-weight: bold;
}
.scroll {
  margin: 30px 18px;
  width: 95%;
}
</style>
<style>
  .systemContent a{
    color: #0097d0;
    cursor: pointer;
    text-decoration: none;
  }

  .systemContent a:hover {
    color: #0bbbfd;
  }
</style>
