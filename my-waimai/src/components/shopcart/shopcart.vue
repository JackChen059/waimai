<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left clearfix">
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
  </div>
</template>

<script>
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
      }
    },
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
    background-color:#141d27;
    .content{
      display:flex;
      height:100%;
      .content-left{
        position: relative;
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
          background-color:rgb(240,20,20);
        }
        .logo-wrapper{
          width:56px;
          height:56px;
          margin-left:12px;
          margin-top:-9px;
          padding-top:6px;
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