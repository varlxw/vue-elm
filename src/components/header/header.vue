<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{ seller.name }}</span>
        </div>
        <div class="description">
          {{ seller.description}}/{{ seller.deliveryTime }}分钟
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{ seller.supports[0].description }}</span>
        </div>
      </div>
      <div @click="showDatail" v-if="seller.supports" class="support-count">
        <span class="count">{{ seller.supports.length }}个</span>
        <i></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDatail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i></i>
    </div>
    <div class="bgc">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="datailShow" class="datail">
        <div class="datail-wrapper clearfix">
          <div class="datail-main">
            <h1 class="name">{{ seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item, index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="datail-close" @click="hidDatail">
          <i>X</i>
        </div>
      </div>
    </transition>

  </div>
</template>
<script>
  import star from '../star/star'
  export default {
    data() {
      return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        datailShow: false
      }
    },
    methods: {
      showDatail() {
        this.datailShow = true
      },
      hidDatail() {
        this.datailShow = false
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      star
    }
  }
</script>
<style lang="scss" rel="stylesheet/scss">
  .header {
    position: relative;
    background: rgba(7, 17, 27, 0.5);
    overflow: hidden;
    color: #fff;
    .content-wrapper {
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;
      .avatar {
        display: inline-block;
        vertical-align: top;
        img {
          border-radius: 2px;
        }
      }
      .content {
        display: inline-block;
        margin-left: 16px;
        .title {
          margin: 2px 0 8px 0;

          .brand {
            display: inline-block;
            width: 30px;
            height: 18px;
            vertical-align: top;
            background: url('./brand@2x.png') no-repeat;
            -webkit-background-size: 30px 18px;
            background-size: 30px 18px;
          }
          .name {
            margin-left: 6px;
            font-size: 16px;
            line-height: 18px;
            font-weight: bold;
          }
        }
        .description {
          margin-bottom: 10px;
          line-height: 12px;
          font-size: 12px;
        }
        .support {
          .icon {
            vertical-align: top;
            display: inline-block;
            width: 12px;
            height: 12px;
            margin-right: 4px;
            -webkit-background-size: 12px 12px;
            background-size: 12px 12px;
            background-repeat: no-repeat;
            &.decrease {
              background-image: url('./decrease_1@2x.png');
            }
            &.discount {
              background-image: url('./discount_1@2x.png');
            }
            &.guarantee {
              background-image: url('./guarantee_1@2x.png');
            }
            &.invoice {
              background-image: url('./invoice_1@2x.png');
            }
            &.special {
              background-image: url('./special_1@2x.png');
            }
          }
          .text {
            line-height: 12px;
            font-size: 10PX;
          }
        }
      }
      .support-count {
        position: absolute;
        right: 12px;
        bottom: 18px;
        padding: 0 8px;
        height: 24px;
        line-height: 24px;
        border-radius: 14px;
        background: rgba(0, 0, 0, 0.2);
        text-align: center;
        .count {
          font-size: 10px;
        }
        i {
          font-style: normal;
          margin-left: 3px;
          font-size: 13px;
          &:before {
            content: '>';
          }
        }
      }
    }
    .bulletin-wrapper {
      height: 28px;
      line-height: 28px;
      padding: 0 22px 0 12px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      position: relative;
      background: rgba(7, 17, 27, 0.2);
      .bulletin-title {
        display: inline-block;
        width: 22px;
        height: 12px;
        margin-top: 8px;
        background: url('./bulletin@2x.png');
        -webkit-background-size: 22px 12px;
        background-size: 22px 12px;
        background-repeat: no-repeat;
      }
      .bulletin-text {
        font-size: 10px;
        font-weight: 200;
        margin: 0 4px;
        vertical-align: top;
      }
      i {

        font-style: normal;
        font-size: 13px;
        position: absolute;
        right: 8px;
        &:before {
          content: '>';
        }
      }
    }
    .bgc {
      width: 100%;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
      filter: blur(10px);
    }
    .datail {
      position: fixed;
      z-index: 100;
      width: 100%;
      height: 100%;
      left: 0;
      top: 0;
      overflow: auto;
      background: rgba(7, 17, 27, 0.8);
      bg
      &.fade-enter-active, &.fade-leave-active {
        transition: all .5s
      }
      &.fade-enter, &.fade-leave-active {
        opacity: 0;
      }
      .datail-wrapper {
        min-height: 100%;
        width: 100%;
        .datail-main {
          margin-top: 64px;
          padding-bottom: 64px;
          .name {
            line-height: 16px;
            text-align: center;
            font-size: 16px;
            font-weight: 700;
          }
          .star-wrapper {
            margin-top: 18px;
            padding: 2px 0;
            text-align: center;
          }
          .title {
            display: flex;
            width: 80%;
            margin: 28px auto 24px auto;
            .line {
              flex: 1;
              position: relative;
              top: -6px;
              border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            }
            .text {
              padding: 0 12px;
              font-weight: 700;
              font-size: 14px;
            }
          }
          .supports {
            width: 80%;
            margin: 0 auto;
            .support-item {
              padding: 0 12px;
              margin-bottom: 12px;
              &:last-child {
                margin-bottom: 0;
              }
              .icon {
                display: inline-block;
                width: 16px;
                height: 16px;
                vertical-align: top;
                margin-right: 6px;
                background-size: 16px 16px;
                background-repeat: no-repeat;
                &.decrease {
                  background-image: url('./decrease_2@2x.png');
                }
                &.discount {
                  background-image: url('./discount_2@2x.png');
                }
                &.guarantee {
                  background-image: url('./guarantee_2@2x.png');
                }
                &.invoice {
                  background-image: url('./invoice_2@2x.png');
                }
                &.special {
                  background-image: url('./special_2@2x.png');
                }
              }
              .text {
                line-height: 16px;
                font-size: 12px;
              }
            }
          }
          .bulletin {
            width: 80%;
            margin: 0 auto;
            .content {
              padding: 0 12px;
              line-height: 24px;
              font-size: 12px;
            }
          }
        }
      }
      .datail-close {
        position: relative;
        width: 32px;
        height: 32px;
        margin: -60px auto 0 auto;
        clear: both;
        font-size: 20px;
        i {
          margin: 0 8px;
          font-style: normal;
          color: #ccc;
        }
      }
    }
  }
</style>
