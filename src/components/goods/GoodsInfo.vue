<template>
  <div class="goodsinfo-contianer">
    <transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
      <div class="ball" v-show="ballFlag" ref="ball"></div>
    </transition>

    <!-- 商品轮播图区域 -->
    <div class="mui-card">
      <div class="mui-card-content">
        <div class="mui-card-content-inner">
          <mt-swipe :auto="4000">
            <mt-swipe-item v-for="item in lunbotu" :key="item.src">
              <img :src="item.src" alt="加载失败" />
            </mt-swipe-item>
          </mt-swipe>
        </div>
      </div>
    </div>

    <!-- 商品购买区域 -->
    <div class="mui-card">
      <div class="mui-card-header">{{goodsinfo.title}}</div>
      <div class="mui-card-content">
        <div class="mui-card-content-inner">
          <p class="price">
            市场价：
            <del>￥{{goodsinfo.market_price}}</del>&nbsp;&nbsp;销售价：
            <span class="now_price">￥{{goodsinfo.sell_price}}</span>
          </p>
          <p>
            购买数量：
            <numbox @getcount="getSelectedCount" :max="goodsinfo.stock_quantity"></numbox>
          </p>
          <p>
            <mt-button type="primary" size="small">立即购买</mt-button>
            <mt-button type="danger" size="small" @click="addToShopCar">加入购物车</mt-button>
          </p>
        </div>
      </div>
    </div>

    <!-- 商品参数区域 -->
    <div class="mui-card">
      <div class="mui-card-header">商品参数</div>
      <div class="mui-card-content">
        <div class="mui-card-content-inner">
          <p>商品货号：{{goodsinfo.goods_no}}</p>
          <p>库存情况：{{goodsinfo.stock_quantity}}件</p>
          <p>上架时间：{{goodsinfo.add_time | dateFormat}}</p>
        </div>
      </div>
      <div class="mui-card-footer">
        <mt-button type="primary" size="large" plain @click="goDesc(id)">图文介绍</mt-button>
        <mt-button type="danger" size="large" plain @click="goComment(id)">商品评论</mt-button>
      </div>
    </div>
  </div>
</template>

<script>
  import numbox from "../subcomponents/goodsinfo_numbox.vue";
  export default {
    data() {
      return {
        id: this.$route.params.id,
        lunbotu: [],
        goodsinfo: {},
        ballFlag: false, // 控制小球隐藏与显示的标识符
        selectedCount: 1 // 保存用户选中的商品数量， 默认，认为用户买1个
      };
    },
    created() {
      this.getLunbotu();
      this.getGoodsInfo();
    },
    methods: {
      getLunbotu() {
        this.$http.get("api/getthumimages/" + this.id).then(result => {
          if (result.body.status === 0) {
            this.lunbotu = result.body.message;
          }
        });
      },
      getGoodsInfo() {
        this.$http.get("api/goods/getinfo/" + this.id).then(result => {
          if (result.body.status === 0) {
            this.goodsinfo = result.body.message[0];
          }
        });
      },
      goDesc(id) {
        //编程式导航到图文介绍页面
        this.$router.push({ name: "goodsdesc", params: { id } });
      },
      goComment(id) {
        //编程式导航到评论页面
        this.$router.push({ name: "goodscomment", params: { id } });
      },
      addToShopCar() {
        this.ballFlag = !this.ballFlag;
        var goodsinfo = {
          id: this.id,
          count: this.selectedCount,
          price: this.goodsinfo.sell_price,
          selected: true
        };
        // 调用 store 中的 mutations 来将商品加入购物车
        this.$store.commit("addToCar", goodsinfo);
      },
      beforeEnter(el) {
        el.style.transform = "translate(0, 0)";
      },
      enter(el, done) {
        el.offsetWidth;

        // 小球动画优化思路：
        // 1. 先分析导致 动画 不准确的 本质原因： 我们把 小球 最终 位移到的 位置，已经局限在了某一分辨率下的 滚动条未滚动的情况下；
        // 2. 只要分辨率和 测试的时候不一样，或者 滚动条有一定的滚动距离之后， 问题就出现了；
        // 3. 因此，我们经过分析，得到结论： 不能把 位置的 横纵坐标 直接写死了，而是应该 根据不同情况，动态计算这个坐标值；
        // 4. 经过分析，得出解题思路： 先得到 徽标的 横纵 坐标，再得到 小球的 横纵坐标，然后 让 y 值 求差， x 值也求 差，得到 的结果，就是横纵坐标要位移的距离
        // 5. 如何 获取 徽标和小球的 位置？？？   domObject.getBoundingClientRect()

        // 获取小球的 在页面中的位置
        const ballPosition = this.$refs.ball.getBoundingClientRect();
        // 获取 徽标 在页面中的位置
        const badgePosition = document
          .getElementById("badge")
          .getBoundingClientRect();

        const xDist = badgePosition.left - ballPosition.left;
        const yDist = badgePosition.top - ballPosition.top;

        el.style.transform = `translate(${xDist}px, ${yDist}px)`;
        el.style.transition = "all 0.5s cubic-bezier(.01,.13,.09,.42)";
        done();
      },
      afterEnter(el) {
        this.ballFlag = !this.ballFlag;
      },
      getSelectedCount(count) {
        // 当子组件把 选中的数量传递给父组件的时候，把选中的值保存到 data 上
        this.selectedCount = count;
        // console.log("父组件拿到的数量值为： " + this.selectedCount);
      }
    },
    components: {
      numbox
    }
  };
</script>

<style lang="scss" scoped>
.goodsinfo-contianer {
  background-color: #eee;
  overflow: hidden;

  .now_price {
    color: red;
    font-size: 16px;
    font-weight: bold;
  }
  .mui-card-footer {
    display: block;
    button {
      margin: 12px 0;
    }
  }

  .ball {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: red;
    position: absolute;
    z-index: 99;
    top: 391px;
    left: 152px;
  }
}

.mint-swipe {
  height: 200px;
  .mint-swipe-item {
    text-align: center;
    img {
      height: 100%;
    }
  }
}
</style>