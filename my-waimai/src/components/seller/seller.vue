<template>
  <div class="seller" ref="sellerScroll">
    <div class="contentAll">
      <div class="sale clearfix">
        <div class="saleInfor pull-left">
          <h3>{{seller.name}}</h3>
          <star :star="seller.score" :size="36" class="pull-left star"></star>
          <span>({{seller.ratingCount}})</span>
          <span>已售{{seller.sellCount}}单</span>
        </div>
        <div @click="change" v-bind:class="{'coll':A,'collect':!A}">
          <span class="icon-favorite"></span>
          <p v-text="Ptext"></p>
        </div>
      </div>

      <ul class="price">
          <li>
            <span>起送价</span>
            <p>{{seller.minPrice}}<i>元</i></p>
          </li>
          <li>
            <span>商家配送</span>
            <p>{{seller.deliveryPrice}}<i>元</i></p>
          </li>
          <li>
            <span>平均配送时间</span>
            <p>{{seller.deliveryTime}}<i>分钟</i></p>
          </li>
      </ul>

      <div class="segmentation"></div>

      <div class="notice">
        <h3>公告与活动</h3>
        <div>
          {{seller.bulletin}}
        </div>
      </div>

      <div>
        <supports v-if="seller.supports" :supports="seller.supports" :className="SbigSup"></supports>
      </div>

      <div class="segmentation"></div>

      <div class="scene">
        <h3>商家实景</h3>
        <div class="pic" ref="Xmove">
        <ul>
          <li v-for="item in seller.pics"><img :src="item"></li>
        </ul>
        </div>
      </div>

      <div class="segmentation"></div>

      <div class="information">
        <h3>商家信息</h3>
        <ul>
            <li v-for="item in seller.infos">{{item}}</li>
        </ul>
      </div>
    </div>


    <!-- <div class="shopcart-wrapper">
      <shopcart :delivery="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    </div> -->
  </div>
</template>

<script>

  import shopcart from"../shopcart/shopcart.vue"
  import bScroll from "better-scroll"
  import star from"../star/star.vue"
  import supports from"../supports/supports.vue"

  export default {
    data(){
      return{
        seller: Object,
        SbigSup:'SbigSup',
        A:false,
        Ptext:'已收藏'
      }
    },
    created(){
      this.$http.get('/api/seller').then(response => {
        this.seller = response.body.seller
        this.$nextTick(() => {
          this._initScroll();
        })
      })
    },
    methods:{
      _initScroll(){
        this.Xmove = new bScroll(this.$refs.Xmove,{
          click:true,
          scrollX:'true'
        });
        this.sellerScroll = new bScroll(this.$refs.sellerScroll,{
          click:true,
          starY:true
        })
        this.sellerScroll.refresh()
      },
      change(){
        this.A = !this.A
        if(this.A){
          this.Ptext='未收藏'
        }else{
          this.Ptext='已收藏'
        }
      }
    },
    components:{
      star:star,
      supports:supports,
      shopcart:shopcart
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .seller{
    position: fixed;
    top:174px;
    height:100%;
    width:100%;
    overflow:hidden;
    .contentAll{
      padding-bottom:174px;
      .sale{
        position: relative;
        margin:0 18px;
        padding:18px 0;
        border-bottom:1px solid rgba(7,17,27,0.1);
        .saleInfor{
          display:inline-block;
          h3{
            margin-bottom:8px;
            font-size:14px;
            color:rgb(7,17,27);
            line-height: 14px;
          }
          span{
            vertical-align: top;
            font-size:10px;
            color:rgb(77,85,93);
            line-height:14px;
            &:nth-child(3){
              margin-right: 12px;
              margin-left:8px;
            }
          }
        }
        .collect,.coll{
          display:inline-block;
          position: absolute;
          top: 18px;
          right: 18px;
          text-align:center;

          span{
            font-size:24px;
            color:rgb(240,20,20);
            line-height:24px;
          }
          p{
            font-size:10px;
            color:rgb(77,85,93);
            line-height:10px;
          }
        }
        .coll>span{
              color:rgba(7,17,27,0.1);
            }
      }
      .price{
          padding:18px 0;
          overflow:auto;
          box-sizing: border-box;
          li{
            box-sizing: border-box;
            float: left;
            width:33.33%;
            text-align:center;
            &:nth-child(1){
              border-right:1px solid rgba(7,17,27,0.1);
            }
            &:nth-child(2){
              border-right:1px solid rgba(7,17,27,0.1);
            }
            span{
              font-size:10px;
              color:rgb(147,153,159);
              line-height:12px;
              margin-bottom:4px;
            }
            p{
              font-size:24px;
              font-weight:200;
              color:rgb(7,17,27);
              line-height:24px;
              i{
                font-size:10px;
              }
            }
        }
      }
      .segmentation{
        box-sizing:border-box;
        height:16px;
        background:#f3f5f7;
        border-top:1px solid rgba(7,17,27,.1);
        border-bottom:1px solid rgba(7,17,27,.1);
      }

      .notice{
        padding:18px 18px 16px;
        h3{
          margin-bottom:8px;
          font-size:14px;
          color:rgb(7,17,27);
          line-height: 14px;
        }
        div{
          padding:8px 12px 0;
          font-size:12px;
          font-weight:200;
          color:rgb(240,20,20);
          line-height:24px;
        }
      }

      .scene{
        padding:18px 0 18px 18px;
        h3{
          margin-bottom:12px;
          font-size:14px;
          color:rgb(7,17,27);
          line-height: 14px;
        }
        .pic{
          position: relative;
          height:90px;
          width: 100%;
          white-space:nowrap;
          overflow:hidden;
          ul{
              position: absolute;
              top: 0;
              left: 0;
              font-size: 0;
            li{
              width:120px;
              height:100%;
              display: inline-block;
              margin-right:6px;
              img{
                height:100%;
                width:100%;
              }
            }
          }
        }
      }

      .information{
        padding:18px 0 0 18px;
        margin-bottom:47px;
        h3{
          margin-bottom:12px;
          font-size:14px;
          color:rgb(7,17,27);
          line-height: 14px;
        }
        ul{
          li{
            box-sizing: border-box;
            padding:16px 12px;
            font-weight:200;
            font-size:12px;
            line-height:16px;
            border-top:1px solid rgba(7,17,27,0.1);
          }
        }
      }
    }

  }
</style>