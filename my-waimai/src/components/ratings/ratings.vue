<template>
  <div class="ratings" ref="ratings">
    <div class="contentAll">
      <div class="totalScore clearfix">
        <div class="integrated pull-left">
          <dt>{{seller.score}}</dt>
          <dl>
            <dd class="interScore">综合评分</dd>
            <dd class="compare">高于周边商家{{seller.rankRate}}%</dd>
          </dl>
        </div>
        <div class="starScore pull-left">
          <div >
            <span class="pull-left">服务态度:</span><star :star="seller.serviceScore" :size="36" class="pull-left star"></star><span class="scoreNum">{{seller.serviceScore}}</span>
          </div>
          <div>
            <span class="pull-left">食物评分:</span><star :star="seller.foodScore" :size="36" class="pull-left star"></star><span class="scoreNum">{{seller.foodScore}}</span>
          </div>
          <p>送达时间<span>{{seller.deliveryTime}}分钟</span></p>
        </div>
      </div>

      <div class="segmentation"></div>

      <div class="evaluation">
        <div class="satisfaction clearfix" v-if="ratings">
          <div class="pull-left" @click="all($event)">
            <span>全部</span>
            <span>{{ratings.length}}</span>
          </div>
          <div class="pull-left" @click="sat($event)">
            <span>满意</span>
            <span>{{sati}}</span>
          </div>
          <div class="pull-left" @click="di($event)">
            <span>不满意</span>
            <span>{{dis}}</span>
          </div>
        </div>
        <div class="segmentTop" >
          <span class="icon-check_circle" :class="{light:tex>-1}" @click="show"></span>
          <span class="iconWord">只看有内容的评价</span>
        </div>
        <div class="segmentCon">
          <ul>
            <li v-for="item in ratings" v-if="item.text.length>tex&&item.rateType>ratetype&&item.rateType<ratetype1" class="clearfix">
            <img :src="item.avatar" class="pull-left">
            <div class="pull-left content">
              <dt>{{item.username}}</dt>
              <dl>
                <star :star="item.score" :size="48" class="pull-left star"></star>
                <span v-if="item.deliveryTime">{{item.deliveryTime}}分钟送达</span>
              </dl>
              <p v-if="item.text!=''">{{item.text}}</p>
              <p v-else>该用户没有留言</p>
              <div class="thumb">
                <span class="icon-thumb_up up pull-left" v-if="item.rateType==0"></span>
                <span class="icon-thumb_down down pull-left" v-if="item.rateType==1"></span>
                <ul class="pull-left">
                  <li v-for="(rem,index) in item.recommend" v-if="index < 3" class="pull-left">{{rem}}</li>
                </ul>
              </div>
            </div>
            </li>
          </ul>
        </div>
      </div>
    </div>


    <!-- <div class="shopcart-wrapper">
      <shopcart :delivery="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    </div> -->
  </div>
</template>

<script>

  import betterScr from "better-scroll"
  import star from"../star/star.vue"
  import shopcart from"../shopcart/shopcart.vue"

  export default {
    props:{
      seller:{
        return:Object
      }
    },
    data(){
      return{
        ratings:Object,
        sati:0,
        dis:0,
        tex:-1,
        ratetype:-1,
        ratetype1:2
      }
    },
    created(){
      this.$http.get('api/ratings').then(response => {
        this.ratings = response.body.data
        this.satisfied();
        this.dissatisfied();
        this.$nextTick(() =>{
          this._initScroll()
        })
      })
    },
    watch:{
      "tex"(){
        this.$nextTick(() =>{
          this.ratingsScroll.refresh()
        })
      },
      "ratetype"(){
        this.$nextTick(() =>{
          this.ratingsScroll.refresh()
        })
      },
      "ratetype"(){
        this.$nextTick(() =>{
          this.ratingsScroll.refresh()
        })
      }
    },
    methods:{
      _initScroll(){
        this.ratingsScroll = new betterScr(this.$refs.ratings,{
          click: true,
          // startY: true
        });
        this.ratingsScroll.refresh()
      },
      satisfied(){
        let sati = 0;
        for(let i=0;i<this.ratings.length;i++){
          if(this.ratings[i].rateType == 0){
            this.sati+=1
          }
        }
      },
      dissatisfied(){
        let dis = 0
        for(let i=0;i<this.ratings.length;i++){
          if(this.ratings[i].rateType == 1){
            this.dis+=1
          }
        }
      },
      show(){
        if(this.tex === -1){
          this.tex = 0
        }else{
          this.tex = -1
        }
      },
      di(event){
        if(!event._constructed){
          return
        }
        this.ratetype = 0
        this.ratetype1 = 2
      },
      sat(event){
        if(!event._constructed){
          return
        }
        this.ratetype = -1
        this.ratetype1 = 1
      },
      all(event){
        if(!event._constructed){
          return
        }
        this.ratetype = -1
        this.ratetype1 = 2
      }
    },
    components:{
      star:star,
      shopcart:shopcart
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import './../common/common.scss';
  .ratings{
    position: fixed;
    top:174px;
    height:100%;
    width:100%;
    overflow:hidden;
    .contentAll{
      padding-bottom:125px;
      .totalScore{
        padding:18px 24px 18px 0;
        display:flex;
        .integrated{
          flex:1;
          position: relative;
          // width:36.6%;
          text-align:center;
          &:after{
            display:block;
            content: '';
            position: absolute;
            right: 0;
            top: 0;
            height:100%;
            width:1px;
            background:rgba(147,153,159,0.1);
          }
          dt{
            font-size:24px;
            color:rgb(255,153,0);
            line-height:28px;
          }
          dl{
            .interScore{
              margin-top:6px;
              font-size:12px;
              color:rgb(7,17,27);
              line-height:12px;
            }
            .compare{
              margin-top:8px;
              margin-bottom:6px;
              font-size:10px;
              color:rgb(147,153,159);
              line-height:10px;
            }
          }
        }
        .starScore{
          box-sizing: border-box;
          flex:0 0 237.5px;
          padding-left:24px;
          div{
            span{
              font-size:12px;
              color:rgb(7,17,27);
              line-height:18px;
            }
            .star{
              margin:0 12px;
            }
            .scoreNum{
              margin: 0;
              line-height:18px;
              color:rgb(255,153,0);
            }
          }
          p{
            font-size:12px;
            color:rgb(7,17,27);
            line-height:19px;
            span{
              margin-left:12px;
              color:rgb(147,153,159);
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

      .satisfaction{
        box-sizing: border-box;
        padding:18px 0;
        margin: 0 18px;
        font-size:12px;
        border-bottom:1px solid rgba(7,17,27,0.1);
        div{
          padding:8px 12px;
          margin-right: 8px;
          border-radius: 2px;
          &:nth-child(1){
            background-color:rgb(0,160,220);
          }
          &:nth-child(2){
            background-color:rgba(0,160,220,0.2);
          }
          &:nth-child(3){
            background-color:rgba(77,85,93,0.2);
          }
          // &:active{
          //   background-color:pink;
          // }
        }
      }

      .segmentTop{
        padding:12px 18px;
        font-size:0;
        border-bottom:1px solid rgba(7,17,27,0.2);
        color:rgb(147,153,159);
        span{
          font-size:24px;
          line-height:24px;
          margin-right:4px;
          &.light{
            background-color:rgb(255,255,255);
            color:rgb(0,160,220);
          }
        }
        .iconWord{
          vertical-align:top;
          font-size:12px;
          line-height:24px;
        }
      }

      .segmentCon{
        margin: 0 18px 47px;
        ul{
          font-size:0;
          li{
            display:flex;
            border-bottom:1px solid rgba(7,17,27,0.1);
            padding:18px 0;
            img{
              flex:0 0 28px;
              height:28px;
              width:28px;
              border-radius:50%;
            }
            .content{
              flex:1;
              padding-left:12px;
              dt{
                font-size:10px;
                color:rgb(7,17,27);
                line-height:12px;
                margin-bottom:4px;
              }
              dl{
                display: inline-block;
                vertical-align: top;
                margin-bottom:6px;
                .star{
                  margin-right:4px;
                }
                span{
                  font-size:10px;
                  font-weight:200;
                  line-height:12px;
                  color:rgb(147,153,159);
                }
              }
              p{
                font-size:12px;
                color:rgb(7,17,27);
                line-height:18px;
              }
              .thumb{
                margin-top:8px;
                span{
                  font-size:12px;
                  line-height:16px;
                  &.up{
                    color:rgb(0,160,220);
                  }
                  &.down{
                    color:rgb(183,187,191);
                  }
                }
                ul{
                  li{
                    margin-left:8px;
                    padding:0 6px;
                    max-width:63px;
                    font-size:9px;
                    line-height:16px;
                    text-overflow: ellipsis;
                    white-space:nowrap;
                    overflow:hidden;
                    border-radius:2px;
                    border:1px solid rgba(7,17,27,0.1);
                    color:rgb(147,153,159);
                  }
                }
              }
            }
          }
        }
      }

    }
  }

</style>