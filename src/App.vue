<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <div class="tab border_1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to= '/ratings'>评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to= '/seller'>商家</router-link>
      </div>
    </div>
    <div class="content">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import header from './components/header/header'
let ERR_OK = 0
export default {
  data() {
    return {
      seller: {}
    }
  },
  created: function () {
    this.$http.get('/api/seller').then((response) => {
      // success
      response = response.body
      if (response.errno === ERR_OK){
        this.seller = response.data
      }
    }, response => {
      // error
    })
  },
  components: {
    'v-header': header
  }
}
</script>

<style lang= "scss" rel="stylesheet/scss">
  /*@import './common/common.scss';*/
  #app{
    .tab{
      display: flex;
      width: 100%;
      height: 40px;
      line-height: 40px;
      .tab-item{
        flex: 1;
        text-align: center;
        a{
        display: block;
        font-size: 14px;
        color: rgb(77,85,93);
        }
        a.active{
            color: rgb(240,20,20);
          }
      }
    }
  }
</style>
