<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <router-link tag="div" class="tab-item" to="/goods">
        <span class="tab-link">商品</span>
      </router-link>
      <router-link tag="div" class="tab-item" to="/ratings">
        <span class="tab-link">评论</span>
      </router-link>
      <router-link tag="div" class="tab-item" to="/seller">
        <span class="tab-link">商家</span>
      </router-link>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import header from 'components/header/header.vue';
import {urlParse} from 'common/js/util';
const ERR_OK = 0;
export default {
  data() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse();
          return queryParam.id;
        })()
      }
    };
  },
  created() {
    this.$http.get('/api/seller?id=' + this.seller.id).then((res) => {
      res = res.body;
      if (res.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, res.data);
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "./common/stylus/mixin.styl"
#app
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      .tab-link
        font-size: 14px
        color: rgb(77, 85, 93)
      &.router-link-active
        .tab-link
         color: rgb(240, 20, 20)
</style>
