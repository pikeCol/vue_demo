<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <a v-link="{path:'/goods'}">商品</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/ratings'}">评论</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/seller'}">商家</a>
      </div>
    </div>
    <router-view :seller="seller" keep-alive></router-view>
  </div>
</template>

<script type = "text/ecmascript-6">
  import header from './components/header/header';
  import {urlParse} from 'common/js/util';
  const ERR_OK = 0;
  export default {
    data () {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            return queryParam.id;
          })()
        }
      };
    },
    created () {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          // this.seller = response.data;
          // 拓展 seller
          this.seller = Object.assign({}, this.seller, response.data);
          console.log(this.seller.id);
        };
      });
    },
    components: {
      'v-header': header
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import './common/stylus/mixin.styl'
  .tab 
    display: flex
    border-1px(rgba(7, 17, 27, 0.1))
    width: 100%
    .tab-item
      flex: 1
      line-height: 40px
      height: 40px
      text-align: center
      cursor: pointer
      & > a
        display: block
        height: 40px
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)
</style>
