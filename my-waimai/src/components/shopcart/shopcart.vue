<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left clearfix" @click="toggleList">
        <div class="logo-wrapper pull-left">
          <div class="logo-car icon-shopping_cart" :class="{highlight:totalCount>0}">

          </div>
        </div>
        <div class="number" v-show="totalCount">
          {{totalCount}}
        </div>
        <div class="totalPrice clearfix pull-left">
          <div class="money pull-left">
            ￥{{totalPrice}}元
          </div>
          <div class="deliveryFee pull-left">
            另需配送费￥{{delivery}}元
          </div>
        </div>
      </div>
      <div class="content-right" :class='{enough:totalPrice>=minPrice}'>
        <!-- ￥{{minPrice}}起送 -->
        {{pay}}
      </div>
    </div>

    <transition name="fade">
      <div class="list-mask" @click="headList" v-show="mask"></div>
    </transition>
    <transition name="fold">
      <div class="food-list" v-show="listShow">
        <div class="foodTop clearfix">
          <span class="pull-left title">购物车</span>
          <span class="pull-right remove" @click="move">清空</span>
        </div>
        <div class="list-content" ref="listContent">
          <ul>
            <li v-for="food in selectFoods" class="food">
              <span>{{food.name}}</span>
              <div class="price">
                <span>{{food.price}}</span>
              </div>
              <div class="addcontrol-wrapper">
                  <addcontrol :food="food"></addcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>

  import betterScr from "better-scroll"
  import addcontrol from "../addcontrol/addcontrol.vue"

  export default{
    props:{
      delivery:{
        type:Number,
        default:0
      },
      minPrice:{
        type:Number,
        default:0
      },
      selectFoods:{
        type:Array,
        default(){
          return[
          {
            price: 0,
            count: 0
          }]
        }
      }
    },
    computed:{
      totalPrice(){
        let price = 0;
        this.selectFoods.forEach((food) => {
          price += food.price *food.count
        })
        return price
      },
      totalCount(){
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count
      },
      pay(){
        if(this.totalPrice === 0){
          return `￥${this.minPrice}起送`
          // return `最低配送费￥${totalPrice}`
        }else if(this.totalPrice<this.minPrice){
          let gap = this.minPrice - this.totalPrice
          return `还差￥${gap}元`
        }else{
          return '去结算'
        }
      },
      listShow(){
        if(!this.totalCount){
          this.fold = true;
          this.mask = false;
          return false;
        }
        let show = !this.fold;
        return show;
      }
    },
    components:{
      addcontrol:addcontrol
    },
    data(){
      return{
        fold:true,
        mask:false
      }
    },
    methods:{
      toggleList(){
        if(!this.totalCount){
          this.fold = true;
          return false
        }
        this.fold = !this.fold;
        this.mask = true;
        this.$nextTick(() =>{
          if(!this.scroll){
            this.scroll = new betterScr(this.$refs.listContent,{
              click:true
            })
          }else{
            this.scroll.refresh();
          }
        })
      },
      move(){
        this.selectFoods.forEach((food)=>{
          food.count = 0;
        })
      },
      headList(){
        this.fold = false;
        this.mask = false
      },
    }
  }

</script>

<style lang="scss" scoped>
  @import './../common/common.scss';
  .shopcart{
    position: fixed;
    bottom:0;
    left:0;
    height:47px;
    width:100%;
    z-index:100;
    background-color:#141d27;
    .content{
      display:flex;
      height:100%;
      .content-left{
        position: relative;
        background-color:#141d27;
        flex:1;
        .number{
          position: absolute;
          top: -9px;
          left: 44px;
          padding: 2px 6px;
          font-size:9px;
          border-radius:12px;
          color:rgb(255,255,255);
          font-weight:700;
          line-height:16px;
          z-index:21;
          background-color:rgb(240,20,20);
        }
        .logo-wrapper{
          position: relative;
          width:56px;
          height:56px;
          margin-left:12px;
          margin-top:-9px;
          padding-top:6px;
          z-index:20;
          border-radius: 50%;
          background-color:#141d27;
          .logo-car{
            font-size:24px;
            line-height:44px;
            text-align:center;
            color:rgba(255,255,255,0.4);
            height:44px;
            width:44px;
            margin-right: auto;
            margin-left: auto;
            border-radius: 50%;
            background-color:rgba(255,255,255,0.2);
            &.highlight{
              color:rgb(255,255,255);
              background-color:rgb(0,160,220);
            }
          }
        }
        .totalPrice{
          box-sizing: border-box;
          font-size:16px;
          margin-top:12px;
          margin-bottom:12px;
          font-weight:700;
          color:rgba(255,255,255,0.4);
          line-height:24px;
          .money{
            padding-right:12px;
            position: relative;
            &:after{
              display:block;
              content:'';
              position: absolute;
              top: 0;
              right: 0;
              height:100%;
              width:1px;
              background-color:rgba(255,255,255,0.1);
            }
          }
          .deliveryFee{
            padding-left:12px;
            font-size:10px;
            font-weight:300;
          }
        }
      }
      .content-right{
        flex:0 0 105px;
        width:105px;
        line-height:47px;
        text-align:center;
        font-size:12px;
        font-weight:700;
        color:rgba(255,255,255,0.4);
        background-color:#2b333b;
        &.enough{
          color:#fff;
          background-color:#00b43c;
        }
      }
    }


    .list-mask{
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      background:rgba(7,17,27,.6);
      background-filter: blur(10px);
    }
    .fade-enter-active, .fade-leave-active {
      transition: all .5s;
    }
    .fade-enter, .fade-leave-active {
      opacity:0;
    }

    .fold-enter-active, .fold-leave-active {
      transition: all .5s;
    }
    .fold-enter, .fold-leave-active {
      transform: translate3D(0,100%,0);;
      opacity:0;
    }

    .food-list{
      position: absolute;
      left: 0;
      bottom: 47px;
      width:100%;
      max-height:232px;
      z-index:10;
      overflow:hidden;
      padding-bottom:25px;
      background-color:#fff;
      .foodTop{
        height:40px;
        font-weight:200;
        padding:0 18px;
        line-height:40px;
        background-color:#f3f5f7;
        .title{
          font-size:14px;
          color:rgb(7,17,27);
        }
        .remove{
          font-size:12px;
          color:rgb(0,160,220);
        }
      }
      .list-content{
        padding: 0 18px;
        max-height:217px;
        overflow: hidden;
        background:#fff;
        .food{
          position: relative;
          padding: 12px 0;
          box-sizing: border-box;
          @include after1px(rgba(7, 17, 27,.1));
          .name{
            line-height:24px;
            font-size:14px;
            color:rgb(7,17,27);
          }
          .price{
            position: absolute;
            right:90px;
            bottom:8px;
            line-height: 24px;
            font-size:14px;
            font-weight:700;
            color:rgb(240,20,20);
          }
          .addcontrol-wrapper{
            position: relative;
            right:0;
            bottom: -4px;
          }
        }
      }
    }
  }

  @media screen and (max-width: 375px){
    .money{
      padding-right:6px;
      font-size:12px;
    }
    .deliveryFee{
      padding-left:6px;
    }
    .content-right{
      flex:0 0 90px;
      width:90px;
    }
  }
</style>