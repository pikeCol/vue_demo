<template>
  <div v-show="showFlag" class="food" transition="move" v-el:food>
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image" alt="">
        <div class="back" @click="hide">
          <i class="icon-arrow_lift"></i>
        </div>  
      </div>
      <div class="content">
        <h1 class="title">{{food.name}}</h1>
        <div class="detail">
          <span class="sell-count">月售{{food.sellCount}}份</span>
          <span class="rating">好评率{{food.rating}}%</span>
        </div>
        <div class="price">
          <span class="now">¥{{food.price}}</span><span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontroll-wrapper">
          <cartctroll :food="food"></cartctroll>
        </div>
        <div class="buy" transition="fade" @click.stop.prevent="addFirst($event)" v-show="!food.count || food.count===0">加入购物车</div>
      </div>
      <split v-show="food.info"></split>
      <div class="info" v-show="food.info">
        <h1 class="title">商品信息</h1>
        <p class="text">{{food.info}}</p>
      </div>
      <split v-show="food.info"></split>
      <div class="rating">
        <h1 class="title">商品评价</h1>
        <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
      </div>
    </div>
  </div>
</template>
<script type = "text/ecmascript-6">
  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import cartctroll from 'components/cartctroll/cartctroll';
  import split from 'components/split/split';
  import ratingselect from 'components/ratingselect/ratingselect';
  
  // const POSITIVE = 0;
  // const NEGATIVE = 1;
  const ALL = 2;

  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      show () {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$els.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      hide () {
        this.showFlag = false;
      },
      addFirst (event) {
        if (!event._constructed) {
          return;
        }
        this.$dispatch('cart.add', event.target);
        Vue.set(this.food, 'count', 1);
      }
    },
    data () {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    components: {
      cartctroll,
      split,
      ratingselect
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .food
    position: fixed
    left: 0
    top: 0
    bottom:48px
    z-index: 30
    width: 100%
    background: white
    &.move-transition
      transition: all 0.2s linear
      transform: translate3d(0,0,0)
    &.move-enter, &.move-leave
      transform: translate3d(100%,0,0)
    .image-header
      position: relative
      width: 100%
      height: 0
      padding-top: 100%
      img
        position: absolute
        left: 0
        top: 0
        width: 100%
        height: 100%
      .back
        position: absolute
        top: 10px
        left: 0
        .icon-arrow_lift
          display: block
          padding: 10px
          font-size: 20px
          color: #fff
    .content
      position: relative
      padding: 18px
      .title
        line-height: 14px
        margin-bottom: 8px
        font-size: 14px
        font-weight: 700
        color: rgb(7,17,27)
      .detail
        margin-bottom: 18px
        line-height: 10px
        height: 10px
        font-size: 0
        .sell-count, .rating
          font-size: 10px
          color: rgb(147,153,159)
        .sell-count
          margin-right: 12px
      .price
          font-weight: 700
          line-height: 24px
          .now
            margin-right: 8px
            font-size: 14px
            color: rgb(240, 20, 20)
          .old
            text-decoration: line-through
            font-size: 10px
            color: rgb(147,153,159)
      .cartcontroll-wrapper
        position: absolute
        right: 12px
        bottom: 12px
      .buy
        position: absolute
        right: 18px
        bottom: 18px
        z-index: 10
        line-height: 24px
        height: 24px
        padding: 0 12px
        box-sizing: border-box
        font-size: 10px
        border-radius: 12px
        color: #fff
        background: rgb(0,160,220)
        &.fade-transition
          opacity: 1
          transition: all 0.2s
        &.fade-enter, &.fade-leave
          opacity: 0
    .info
      padding: 18px
      .title
        margin-bottom: 6px
        line-height: 14px
        font-size: 14px
        color: rgb(7,27,27)
      .text
        line-height: 24px
        padding: 0 8px
        font-size: 12px
        color: #4d555d
    .rating
      padding-top: 18px
      .title
        margin-left: 18px
        line-height: 14px
        font-size: 14px
        color: rgb(7,27,27)
</style>