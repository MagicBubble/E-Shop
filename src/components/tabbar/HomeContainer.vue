<template>
  <div>
    <mt-swipe :auto="4000">
      <!-- <mt-swipe-item>
        <img src="../../lunboimg/lb1.jpg" alt="加载失败" />
      </mt-swipe-item>-->
      <mt-swipe-item v-for="item in lunbotuList" :key="item.img">
        <img :src="item.img" alt="加载失败" />
      </mt-swipe-item>
    </mt-swipe>

    <ul class="mui-table-view mui-grid-view mui-grid-9">
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/newsList">
          <!-- <span class="mui-icon mui-icon-home"></span> -->
          <img src="src/images/menu1.png" alt />
          <div class="mui-media-body">新闻资讯</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/photolist">
          <img src="src/images/menu2.png" alt />
          <div class="mui-media-body">图片分享</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/goodslist">
          <img src="src/images/menu3.png" alt />
          <div class="mui-media-body">商品购买</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="src/images/menu4.png" alt />
          <div class="mui-media-body">留言反馈</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="src/images/menu5.png" alt />
          <div class="mui-media-body">视频专区</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="src/images/menu6.png" alt />
          <div class="mui-media-body">联系我们</div>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
  import { Toast } from "mint-ui";

  export default {
    data() {
      return {
        //  此处注意，写相对路径 ../../lunboimg/lb2.jpg 会出现错误，因为服务器从根路径开始找资源
        lunbotuList: [
          { img: "src/lunboimg/lb1.jpg" },
          { img: "src/lunboimg/lb2.jpg" },
          { img: "src/lunboimg/lb3.jpg" }
        ]
      };
    },
    created() {
      // this.getLunbotu();
    },
    methods: {
      // getLunbotu() {
      //   // 获取轮播图数据的方法
      //   this.$http.get("http://vue.studyit.io/api/getlunbo").then(result => {
      //     // console.log(result.body);
      //     if (result.body.status === 0) {
      //       // 成功了
      //       this.lunbotuList = result.body.message;
      //     } else {
      //       // 失败的
      //       Toast("加载轮播图失败。。。");
      //     }
      //   });
      // }
    }
  };
</script>

<style lang="scss" scoped>
.mint-swipe {
  height: 200px;
  // 下面是css写法：
  // .mint-swipe-item:nth-child(2) {
  //   background-color: gray;
  // }
  // 下面是scss写法
  .mint-swipe-item {
    &:nth-child(1) {
      //  交集选择器，如果是 :nth-child(1) 则为后代选择器
      background-color: red;
    }
    &:nth-child(2) {
      //  交集选择器，如果是 :nth-child(1) 则为后代选择器
      background-color: rgb(85, 0, 255);
    }
    &:nth-child(3) {
      //  交集选择器，如果是 :nth-child(1) 则为后代选择器
      background-color: greenyellow;
    }
    img {
      width: 100%;
      height: 100%;
    }
  }
}
.mui-grid-view.mui-grid-9 {
  background-color: white;
  border: none;
  img {
    width: 60px;
    height: 60px;
  }
}
.mui-grid-view.mui-grid-9 .mui-table-view-cell {
  border: 0;
}
.mui-media-body {
  font-size: 13px;
}
</style>
