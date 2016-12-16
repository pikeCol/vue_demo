<template>
  <div class="goods">
    <div class="menu-wraper">
      <ul>
        <li v-for="item in goods">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="footer-wraper">i am </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: {}
      };
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
        }
      });
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .goods
    display: flex
    overflow: hidden
    position: absolute
    top: 174px
    width: 100%
    bottom: 46px
    .menu-wraper
      flex: 0 0 80px
      width: 80px
      background: #f4f5f6
    .footer-wraper
      flex: 1
      
</style>