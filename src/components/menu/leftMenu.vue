<template>
    <div class="leftMenuBox">
        <div class="menuTitle">{{menuTitle}}</div>
        <div v-for="(item,i) in btnTitle" :key="i">
            <p @click="index=i" style="font-size:13px" :style="index==i?'background: rgba(255, 254, 254, 0.651)':''">{{item}}</p>
        </div>
    </div>
</template>

<script>
export default {
  //btnTitle为一个字面量对象
  props: ["menuTitle", "btnTitle", "pageIndex"],
  data() {
    return {
      index: this.pageIndex,
      key:'leftMenu'
    };
  },
  watch: {
    index: function(newValue) {
      this.$emit("update:pageIndex", newValue);
    }
  }
};
</script>

<style scoped>
.leftMenuBox {
  /*background: rgba(24, 24, 24, 0.671);*/
  background: rgba(56, 53, 53, 0.671);
  width: 150px;
  display: flex;
  flex-direction: column;
  padding-top: 0px;
  border-radius: 5px;
}
.leftMenuBox p {
  height: 60px;
  line-height: 60px;
  cursor: pointer;
  margin: 0;
  transition: background 0.5s;
  color: white;
  font-size: 18px;
}
.leftMenuBox p:hover {
  background: rgba(255, 254, 254, 0.651);
}
.menuTitle {
  /*background: rgba(0, 0, 0, 0.418);*/
  background: rgba(31, 30, 30, 0.9);
  border-bottom: 2px solid wheat;
  height: 60px;
  width: 150px;
  line-height: 60px;
  font-size: 15px;
  font-weight: bold;
  color: wheat;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}
</style>
