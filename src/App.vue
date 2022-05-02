<template>
  <div class="app-contianer">
    <mt-header fixed title="Vue项目">
      <span slot="left" @click="goBack" v-show="flag">
        <mt-button icon="back">返回</mt-button>
      </span>
    </mt-header>

    <transition>
      <router-view></router-view>
    </transition>

    <nav class="mui-bar mui-bar-tab">
      <router-link class="mui-tab-item-gcl" to="/home">
        <span class="mui-icon mui-icon-home"></span>
        <span class="mui-tab-label">首页</span>
      </router-link>
      <router-link class="mui-tab-item-gcl" to="/search">
        <span class="mui-icon mui-icon-search"></span>
        <span class="mui-tab-label">搜索</span>
      </router-link>

      <router-link class="mui-tab-item-gcl" to="/shopcar">
        <span class="mui-icon mui-icon-extra mui-icon-extra-cart">
          <span class="mui-badge" id="badge">{{
            $store.getters.getAllCount
          }}</span>
        </span>
        <span class="mui-tab-label">购物车</span>
      </router-link>
      <router-link class="mui-tab-item-gcl" to="/member">
        <span class="mui-icon mui-icon-contact"></span>
        <span class="mui-tab-label">我的</span>
      </router-link>
    </nav>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        flag: false,
      };
    },
    created() {
      this.flag = this.$route.path === "/home" ? false : true;
    },
    methods: {
      goBack() {
        //  点击后退
        this.$router.go(-1);
      },
    },
    watch: {
      "$route.path": function (newVal) {
        if (newVal === "/home") {
          this.flag = false;
        } else {
          this.flag = true;
        }
      },
    },
  };
</script>

<style lang="scss" scoped>
.mint-header {
  z-index: 99;
}
.app-contianer {
  padding-top: 40px;
  padding-bottom: 50px;
  overflow-x: hidden;
}
.mint-header {
  background-image: linear-gradient(to bottom right, blue, purple);
}
.mui-bar-tab .mui-tab-item.mui-active {
  color: rgb(64, 0, 255);
}
.v-enter {
  opacity: 0;
  transform: translateX(100%);
}
.v-leave-to {
  opacity: 0;
  transform: translateX(-100%);
  position: absolute;
}
.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease;
}
//改类名解决 tabbar 点击无法切换的问题
.mui-bar-tab .mui-tab-item-gcl.mui-active {
  color: #4000ff;
}
.mui-bar-tab .mui-tab-item-gcl {
  display: table-cell;
  overflow: hidden;
  width: 1%;
  height: 50px;
  text-align: center;
  vertical-align: middle;
  white-space: nowrap;
  text-overflow: ellipsis;
  color: #929292;
}
.mui-bar-tab .mui-tab-item-gcl .mui-icon {
  top: 3px;
  width: 24px;
  height: 24px;
  padding-top: 0;
  padding-bottom: 0;
}
.mui-bar-tab .mui-tab-item-gcl .mui-icon ~ .mui-tab-label {
  font-size: 11px;
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
