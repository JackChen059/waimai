<template>
  <div class="food" v-show="showFlag" ref="food">
    <div class="foodCon">
      <div class="foodTop">
        <div class="showimg" @click="hide">
          <img :src="food.image">
          <!-- <div class="back">
            <i class="iconfont icon-arrow_lift"></i>
          </div> -->
        </div>
        <div class="foodNa">
          <h2>{{food.name}}</h2>
          <p class="rat">
            <span>月售{{food.sellCount}}份</span>
            <span>好评率{{food.rating}}%</span>
          </p>
          <div class="price">
            <span>￥<span>{{food.price}}</span></span>
            <del v-if="food.oldPrice!=''">￥<span>{{food.oldPrice}}</span></del>
          </div>
          <div class="addTo" @click="addFirst" v-if="!food.count || food.count === 0">
            加入购物车
          </div>
          <div class="addcontrol-wrapper" v-else>
            <addcontrol :food="food"></addcontrol>
          </div>
        </div>
      </div>

      <div class="segmentation"></div>

      <div class="introduction" v-if="food.info!=''">
        <h2>商品介绍</h2>
        <p>{{food.info}}</p>
      </div>

      <div class="segmentation"></div>

      <div class="evaluate">
        <div class="satisfaction clearfix" v-if="food.ratings">
          <p class="pull-left" @click="all">
            <span>全部</span>
            <span>{{food.ratings.length}}</span>
          </p>
          <p class="pull-left" @click="sati">
            <span>推荐</span>
            <span>{{satisfied}}</span>
          </p>
          <p class="pull-left" @click="dis">
            <span>吐槽</span>
            <span>{{dissatisfied}}</span>
          </p>
        </div>

        <div class="segmentTop" >
          <span class="icon-check_circle" :class="{light:tex>-1}" @click="charge"></span>
          <span class="iconWord">只看有内容的评价</span>
        </div>

        <div class="segmentCon">
          <ul>
            <li v-for="item in food.ratings" v-if="item.text.length>tex&&item.rateType>ratetype&&item.rateType<ratetype1">
            <div class="content clearfix">
              <div class="infor pull-right">
                <span>{{item.username}}</span>
                <img :src="item.avatar">
              </div>
              <div class="pull-left time">
                <span>{{formdata(item.rateTime)}}</span>
              </div>
            </div>
            <div class="thumb">
              <span class="icon-thumb_up up pull-left" v-if="item.rateType==0"></span>
              <span class="icon-thumb_down down pull-left" v-if="item.rateType==1"></span>
              <p class="pull-left" v-if="item.text!=''">{{item.text}}</p>
              <p class="pull-left" v-else>该用户没有留言</p>
            </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

  import Vue from "vue"
  import shopcart from"../shopcart/shopcart.vue"
  import addcontrol from "../addcontrol/addcontrol.vue"
  import betterScr from "better-scroll"

  export default{
    props:{
      food:{
        return:Object
      }
    },
    data(){
      return{
        showFlag:false,
        tex:-1,
        ratetype:-1,
        ratetype1:2
      }
    },
    methods:{
      show(){
        this.showFlag = true;
        this.$nextTick(() =>{
          if(!this.scroll){
            this.scroll = new betterScr(this.$refs.food,{
              click:true
            })
          }else{
            this.scroll.refresh();
          }
        })
      },
      hide(){
        this.showFlag = false;
      },
      addFirst(){
        Vue.set(this.food,"count",1)
      },
      charge(){
        if(this.tex === -1){
          this.tex = 0
        }else{
          this.tex = -1
        }
      },
      dis(event){
        if(!event._constructed){
          return
        }
        this.ratetype = 0
        this.ratetype1 = 2
      },
      sati(event){
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
      },
      formdata(date){
        var date = new Date(parseInt(date));

        var year = date.getFullYear();
        var month = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1);
        var day = (date.getDate() < 10 ? '0' + (date.getDate()) : date.getDate());
        var hours = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours());
        var minites = (date.getMinutes() <10 ? '0' + date.getMinutes() : date.getMinutes());
        return year+"-"+month+"-"+day+" "+hours+":"+minites
      },
    },
    computed:{
      satisfied(){
        let sati = 0;
        for(let i=0;i<this.food.ratings.length;i++){
          if(this.food.ratings[i].rateType == 0){
            sati+=1
          }
        }
        return sati
      },
      dissatisfied(){
        let dis = 0
        for(let i=0;i<this.food.ratings.length;i++){
          if(this.food.ratings[i].rateType == 1){
            dis+=1
          }
        }
        return dis
      }
    },
    components:{
      shopcart:shopcart,
      addcontrol:addcontrol,
      // rat:rat
    }
  }
</script>

<style lang="scss" scoped>
  @import './../common/common.scss';
  .food{
    position: fixed;
    top: 0;
    left:0;
    width:100%;
    bottom: 47px;
    z-index:9;
    overflow:hidden;
    background-color:#fff;
    .foodCon{
      position: relative;
      z-index:-1;
      padding-bottom:47px;
      .foodTop{
        .showimg{
          position:relative;
          width:100%;
          height:0;
          padding-top:100%;
          overflow: hidden;
          img{
            position: absolute;
            top:0;
            // left: -100px;
            width:100%;
            height:100%;
          }
          .back{
            position: absolute;
            top: 10px;
            left: 10px;
            background-color:rgba(0,0,0,.9);
            border-radius: 50%;
            .iconfont{
              display:block;
              padding: 10px;
              font-size: 20px;
              color:#fff;
            }
          }
        }
        .foodNa{
          position: relative;
          margin-top:18px;
          padding-left:18px;
          h2{
            font-size:14px;
            font-weight:700;
            color:rgb(7,17,27);
            line-height: 14px;
          }
          .rat{
            margin-top:8px;
            font-size:10px;
            color:rgb(147,153,159);
            line-height:10px;
            span{
              padding-right:12px;
            }
          }
          .price{
            margin-top:18px;
            margin-bottom:18px;
            line-height:24px;
            span{
              font-size:10px;
              color:rgb(240,20,20);
              span{
                font-weight:700;
                font-size:14px
              }
            }
            del{
              font-size:10px;
              color:rgb(147,153,159);
              span{
                font-weight:700;
                color:rgb(147,153,159);
              }
            }
          }
          .addTo{
            position: absolute;
            right: 18px;
            bottom: 20px;
            width:74px;
            height:24px;
            // padding:6px 0;
            font-size:10px;
            color:rgb(255,255,255);
            line-height:24px;
            border-radius: 12px;
            background-color:rgb(0,160,220);
            text-align:center;
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

      .introduction{
        padding:18px;
        h2{
          font-size:14px;
          color:rgb(7,17,27);
          line-height: 14px;
        }
        p{
          padding:6px 8px 0;
          line-height:24px;
          font-size:12px;
          font-weight:200;
          color:rgb(77,85,93);
        }
      }

      .evaluate{
        width:100%;
        .satisfaction{
          box-sizing: border-box;
          padding:18px 0;
          margin: 0 18px;
          font-size:12px;
          border-bottom:1px solid rgba(7,17,27,0.1);
          p{
            display:line-block;
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
              border-bottom:1px solid rgba(7,17,27,0.1);
              padding:18px 0;
              .content{
                // padding-left:12px;
                width:100%;
                .infor{
                  img{
                    width:12px;
                    height:12px;
                    border-radius:50%;
                  }
                  span{
                    font-size:10px;
                    color:rgb(147,153,159);
                    line-height:24px;
                    margin-right:6px;
                  }
                }
                .time{
                  font-size:10px;
                  color:rgb(147,153,159);
                  line-height:24px;
                }
              }
              .thumb{
                // margin-top:8px;
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
                p{
                  font-size:12px;
                  padding-left:4px;
                  color:rgb(7,17,27);
                  line-height:16px;
                }
              }
            }
          }
        }
      }
    }
  }
</style>