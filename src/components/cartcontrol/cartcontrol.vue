<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart"><i>-</i></div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart">+</div>
  </div>
</template>
<script>
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event){
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
      },
      decreaseCart(event){
        if (!event._constructed) {
          return
        }
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>
<style lang="scss" rel="stylesheet/scss">
  .cartcontrol {
    font-size: 0;
    height: 28px;
    .cart-decrease {
      display: inline-block;
      text-align: center;
      &.move-enter-active, &.move-leave-active {
        transition: all .3s linear;
      }
      &.move-enter, &.move-leave-active {
        opacity: 0;
        transform: translate3D(24px, 0, 0) rotate(180deg);
      }
      i {
        display: inline-block;
        width: 24px;
        height: 12px;
        border-radius: 50%;
        border: 2px solid rgb(0, 160, 220);
        color: rgb(0, 160, 220);
        font-style: normal;
        font-size: 24px;
        padding-bottom: 12px;
      }
    }
    .cart-count {
      display: inline-block;
      width: 12px;
      font-size: 12px;
      line-height: 24px;
      text-align: center;
      padding: 4px 10px 0 10px;
      vertical-align: top;
      color: rgb(147, 153, 159);
    }
    .cart-add {
      display: inline-block;
      width: 24px;
      height: 24px;
      text-align: center;
      font-size: 24px;
      border-radius: 50%;
      border: 2px solid rgb(0, 160, 220);
      background: rgb(0, 160, 220);
      color: #fff;
    }
  }
</style>
