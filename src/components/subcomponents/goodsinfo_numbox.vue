<template>
  <div class="mui-numbox" data-numbox-min="1">
    <button class="mui-btn mui-btn-numbox-minus" type="button">-</button>
    <input
      id="test"
      class="mui-input-numbox"
      type="number"
      value="1"
      @change="countChanged"
      ref="numbox"
    />
    <button class="mui-btn mui-btn-numbox-plus" type="button">+</button>
  </div>
</template>

<script>
  import mui from "../../lib/mui/js/mui.min.js";

  export default {
    mounted() {
      // 初始化数字选择框组件( mui 规定的)
      mui(".mui-numbox").numbox();
    },
    methods: {
      countChanged() {
        // console.log(this.$refs.numbox.value);
        this.$emit("getcount", parseInt(this.$refs.numbox.value));
      }
    },
    props: ["max"],
    watch: {
      //  属性监听
      max: function(newVal, oldVal) {
        //  使用 JS API 设置 numbox 的最大值（参考 mui 官方文档）
        mui(".mui-numbox")
          .numbox()
          .setOption("max", newVal);
      }
    }
  };
</script>

<style lang="scss" scoped>
</style>