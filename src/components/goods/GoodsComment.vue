<template>
  <div class="goodscomment-container">
    <cmtbox :id="$route.params.id"></cmtbox>
  </div>
</template>

<script>
  import cmtbox from "../subcomponents/comment.vue";
  export default {
    components: {
      cmtbox
    }
  };
</script>

<style lang="scss" scoped>
.goodscomment-container {
  padding: 4px;
}
</style>