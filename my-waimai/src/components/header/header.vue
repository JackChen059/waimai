<template>
  <div class="header">

    <div class="content-wrapper clearfix">
      <img :src="seller.avatar" class="logo pull-left">
      <div class="brand pull-left">
        <div class="trade">
          <span class="brandImg"></span>
          <h2>{{seller.name}}</h2>
        </div>
        <span class="deliveryTime">{{seller.description}}</span>
        <span class="deliveryTime">/{{seller.deliveryTime}}分钟送达</span>
        <div class="supports-wrapper">
          <supports v-if="seller.supports" :supports="seller.supports"></supports>
          <div class="count-wrapper" @click="showDetail">
            <span class="pull-right numLi">{{classMap.length}}个<i class="icon-keyboard_arrow_right"></i></span>
          </div>
        </div>
      </div>
    </div>

    <div class="bulletin clearfix" @click="showDetail">
      <span class="bullImg pull-left"></span>
      <p>{{seller.bulletin}}</p>
      <i class="icon-keyboard_arrow_right"></i></span>
    </div>

    <div class="background">
      <img :src="seller.avatar" alt="">
    </div>

    <transition name="fade">
      <div class="detail" v-show="detailShow" @click="closeDetail">
        <div class="det">
          <div class="detWord">
            <h2>{{seller.name}}</h2>
            <div class="star-wrapper">
              <star :star="seller.score" :size="24"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="titleWord">优惠信息</div>
              <div class="line"></div>
            </div>
            <supports v-if="seller.supports" :supports="seller.supports" :className="bigSup"></supports>
            <div class="title">
              <div class="line"></div>
              <div class="titleWord">商家公告</div>
              <div class="line"></div>
            </div>
            <p class="bulletinCon">{{seller.bulletin}}</p>
          </div>
          <div class="close">
            <span class="icon-close"></span>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import supports from"../supports/supports.vue"
  import star from"../star/star.vue"
  export default {
    props:{
      seller:{
        return:Object
      }
    },
    data(){
      return{
        detailShow: false,
        bigSup:'bigSup',
        size:'size'
      }
    },
    methods:{
      showDetail(){
        this.detailShow = true;
      },
      closeDetail(){
        this.detailShow = false;
      }
    },
    created(){
      this.classMap = ['decrease','discount','guarantee','invoice','special']
    },
    components:{
      supports:supports,
      star:star
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import './../common/common.scss';
  .header{
    height:134px;
    position: relative;
    background-color:rgba(7,17,27,0.5);
    filter:blur;
    .content-wrapper{
      padding: 24px 5px 18px 24px;
      .logo{
        float: left;
        display:inline-block;
        height:64px;
        width:64px;
        border-radius: 2px;
      }
      .brand{
        display:inline-block;
        margin-top:2px;
        margin-left:16px;
        .trade{
          .brandImg{
            float: left;
            display:inline-block;
            height:18px;
            width:30px;
            @include bgimg('brand');
            background-size:cover;
          }
          h2{
            color:#fff;
            font-size:16px;
            font-weight:bold;
            line-height:18px;
            margin-left:36px;
          }
        }
        .deliveryTime{
          font-size:10px;
          color:rgb(255,255,255);
          margin-top:8px;
          font-weight:200;
          line-height:12px;
        }
        .supports-wrapper{
          margin-top:10px;
          height:12px;
          overflow:hidden;
          .count-wrapper{
            position: absolute;
            right: 12px;
            bottom:40px;
            .numLi{
              display:inline-block;
              height:24px;
              width:43px;
              font-size:10px;
              color:rgb(255,255,255);
              font-weight:200;
              line-height:24px;
              text-align:center;
              border-radius: 8px;
              background-color:rgba(0,0,0,0.2);
              i{
                line-height:12px;
              }
            }
          }
        }
      }
    }

    .bulletin{
      box-sizing: border-box;
      height:28px;
      padding: 0 12px;
      line-height:28px;
      background-color:rgba(7,17,27,0.2);
      .bullImg{
        display:inline-block;
        height:12px;
        width:22px;
        margin-top:7px;
        @include bgimg('bulletin');
        background-size:cover;
      }
      p{
        font-size:10px;
        padding-left:4px;
        color:rgb(255,255,255);
        font-weight:200;
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
      }
      i{
        position: absolute;
        bottom: 5px;
        right: 0;
        // margin-top:111px;
        color:#fff;
      }
    }

    .background{
      z-index:-1;
      position: absolute;
      top: 0;
      left: 0;
      width:100%;
      height:134px;
      overflow:hidden;
      img{
        width:100%;
        filter:blur(10px);
      }
    }

    .detail{
      z-index:11;
      position: fixed;
      left: 0;
      top: 0;
      height:100%;
      width:100%;
      overflow:auto;
      color:#fff;
      backdrop-filter:blur(10px);
      background:rgba(7,17,27,0.8);
      .det{
        box-sizing: border-box;
        position: relative;
        min-height:100%;
        // padding-bottom:30px;
        .detWord{
          padding:0 32px;
          h2{
            margin-top:64px;
            font-size:16px;
            font-weight:700;
            text-align:center;
          }
          .star-wrapper{
            margin:16px auto 28px auto;
          }
          .title{
            display:flex;
            .line{
              position: relative;
              top:-7px;
              flex:1;
              border-bottom:1px solid rgba(255,255,255,0.2);
            }
            .titleWord{
              padding:0 12px;
              font-size:14px;
            }
          }
          .bulletinCon{
            padding:24px 12px 0;
            line-height:24px;
            font-size:12px;
            font-weight:200;
            color:rgb(255,255,255);
          }
        }
        .close{
          position: absolute;
          bottom:0;
          padding-bottom:79px;
          width:100%;
          font-size:32px;
          text-align:center;
          color:rgba(255,255,255,0.5);
        }
      }
    }
    .fade-enter-active,.fade-leave-active{
      transition:opacity 500ms;
    }
    .fade-leave-to,.fade-enter{
      opacity: 0;
    }
  }

</style>