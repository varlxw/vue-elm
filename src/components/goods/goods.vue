<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="menu-item border_1px">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="food-list">
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
                  <span class="count">月售{{food.sellCount}}</span>
                  <span>好评率{{food.rating}}</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
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
        goods: []
      }
    },
    created: function () {
      this.$http.get('/api/goods').then((response) => {
        // success
        response = response.body
        if (response.errno === ERR_OK) {
          this.goods = response.data
          console.log(this.goods)
        }
      })
    }

  }
</script>
<style lang="scss" rel="stylesheet/scss">
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
            border: 0;
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
          }
        }
      }

    }
  }
</style>
