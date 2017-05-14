<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="item.id" class="menu-item border_1px"
            :class="{current: index===currentIndex}" @click="selectMenu(index,$event)">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="food-list food-list-hook" ref="foodListHook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border_1px">
              <div class="icon">
                <img :src="food.icon" width="57" height="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}</span><span>好评率{{food.rating}}</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old"
                                                                v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food" ></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>
<script>
  import BScroll from 'better-scroll'
  import shopcart from '../shopcart/shopcart'
  import cartcontrol from '../cartcontrol/cartcontrol'
  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    created: function () {
      this.$http.get('/api/goods').then((response) => {
        // success
        response = response.body
        if (response.errno === ERR_OK) {
          this.goods = response.data
          this.$nextTick(() => {
            this._initScroll()
            this._calculateHeight()
          })
        }
      })
    },
    methods: {
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        })
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
//          console.log(this.scrollY)
        })
      },
      _calculateHeight() {
        let foodList = this.$refs.foodListHook
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
//        console.log(this.listHeight)
      },
      selectMenu(index, event) {
//        console.log(index + 'index')
        if (!event._constructed) {
          return
        }
        let foodList = this.$refs.foodListHook
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el, 300)
//        console.log(index)
      }
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
//          console.log(height1 + 'height1')
          let height2 = this.listHeight[i + 1]
//          console.log(height2 + 'height2')
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
//            console.log(i + 'i')
            return i
          }
        }
      },
      selectFoods() {
        let foods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    components: {
      shopcart,
      cartcontrol
    }

  }
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/css/iconfont.css";

  .goods {
    display: flex;
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;
    .menu-wrapper {
      flex: 0 0 80px;
      width: 80px;
      background: #f3f5f7;
      .menu-item {
        display: table;
        width: 56px;
        height: 54px;
        line-height: 14px;
        padding: 0 12px;
        &.current {
          position: relative;
          z-index: 10;
          margin-top: -1px;
          background: #fff;
          font-weight: 700;
        }
        .icon {
          vertical-align: top;
          display: inline-block;
          width: 12px;
          height: 12px;
          margin-right: 2px;
          -webkit-background-size: 12px 12px;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          &.decrease {
            background-image: url('./decrease_3@2x.png');
          }
          &.discount {
            background-image: url('./discount_3@2x.png');
          }
          &.guarantee {
            background-image: url('./guarantee_3@2x.png');
          }
          &.invoice {
            background-image: url('./invoice_3@2x.png');
          }
          &.special {
            background-image: url('./special_3@2x.png');
          }
        }
        .text {
          display: table-cell;
          width: 56px;
          vertical-align: middle;
          font-size: 12px;
        }
      }
    }
    .foods-wrapper {
      flex: 1;
      .food-list {
        .title {
          padding-left: 14px;
          height: 26px;
          line-height: 26px;
          border-left: 2px solid #d9dde1;
          font-size: 12px;
          color: rgb(147, 153, 159);
          background: #f3f5f7;
        }
        .food-item {
          display: flex;
          margin: 18px;
          padding-bottom: 18px;
          &:last-child {
            margin-bottom: 0;
            border-bottom: 0px solid #fff;
          }
          .icon {
            flex: 0 0 57px;
            margin-right: 10px;
          }
          .content {
            flex: 1;
            .name {
              margin: 2px 0 8px 0;
              height: 14px;
              line-height: 14px;
              font-size: 14px;
              color: rgb(7, 17, 27);
              font-weight: 700;
            }
            .desc, .extra {
              line-height: 10px;
              font-size: 10px;
              color: rgb(147, 153, 159);
            }
            .desc {
              line-height: 12px;
              margin-bottom: 8px;
            }
            .extra {
              .count {
                margin-right: 12px;
              }
            }
            .price {
              font-weight: 700;
              line-height: 24px;
              .now {
                margin-right: 8px;
                font-size: 14px;
                color: rgb(240, 20, 20);
              }
              .old {
                text-decoration: line-through;
                font-size: 10px;
                color: rgb(147, 153, 159);
              }
            }
            .cartcontrol-wrapper {
              position: absolute;
              right: 0;
              bottom: 10px;
            }
          }
        }
      }

    }
  }
</style>
