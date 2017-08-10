<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}" @click="_conMove(index,$event)">
          <span class="text">
            <span class="icon" v-if="item.type > 0" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>

    <div class="con-wrapper" ref="conWrapper">
      <ul>
        <li v-for="item in goods" class="food-list-hook">
          <h2>{{item.name}}</h2>
          <ul>
            <li v-for="food in item.foods" class="clearfix" @click="chosefood(food,$event)">
              <img :src="food.icon" class="pull-left">
              <div class="information">
                <dt>{{food.name}}</dt>
                <dl>
                  <dd>{{food.description}}</dd>
                  <dd><span>月售{{food.sellCount}}</span><span>好评率{{food.rating}}%</span></dd>
                </dl>
                <p>￥{{food.price}}<del v-if="food.oldPrice!=''">￥{{food.oldPrice}}</del></p>
                <div class="addcontrol-wrapper">
                  <addcontrol :food="food"></addcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>

    <div class="shopcart-wrapper">
      <shopcart :delivery="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFoods="selectFoods"></shopcart>
    </div>

    <div class="food-wrapper">
      <food :food = "chosefoods" ref="food"></food>
    </div>
  </div>
</template>

<script>

  import food from "../food/food.vue"
  import betterScr from "better-scroll"
  import shopcart from"../shopcart/shopcart.vue"
  import addcontrol from "../addcontrol/addcontrol.vue"

  export default {
    props:{
      seller:{
        return:Object
      }
    },
    data(){
      return{
        goods:[],
        listHeight:[],
        scrollY:0,
        chosefoods:{}
      }
    },
    created(){
      this.$http.get('api/goods').then(response => {
        this.goods = response.body.data
        this.$nextTick(() => {
          this._initScroll();
          this._calculateHeight()
        })
      })
        this.classMap = ['decrease','discount','special']
    },
    methods:{
      _initScroll(){
        this.menuScroll = new betterScr(this.$refs.menuWrapper,{
          click: true
        });
        this.foodScroll = new betterScr(this.$refs.conWrapper,{
          probeType:3,
          click:true
        });
        this.foodScroll.on('scroll',(pos) =>{
          this.scrollY = Math.abs(Math.floor(pos.y));
          // console.log(this.scrollY)
        })
      },
      _calculateHeight(){
        let foodList = this.$refs.conWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for(let i=0;i<foodList.length;i++){
          let item = foodList[i];
          height +=item.clientHeight;
          this.listHeight.push(height);
        }
      },
      _conMove(index,event){
        let foodList = this.$refs.conWrapper.getElementsByClassName('food-list-hook');
        let ele = foodList[index];

        if(!event._constructed){
          return
        }
        // if(event.isTrusted==false){
        //   this.foodScroll.scrollToElement(ele,300);
        // }
          this.foodScroll.scrollToElement(ele,300);
        // console.log(event);
      },
      chosefood(food,event){
        if(!event._constructed){
          return
        }
        this.chosefoods =food;
        this.$refs.food.show();
      }
    },
    computed:{
      currentIndex(){
        for(let i=0;i<this.listHeight.length;i++){
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i+1];
          if(!height2||this.scrollY>=height1&&this.scrollY<height2){
            return i
          }
        }
        return 0;
      },

      selectFoods(){
        let foodList = [];
        this.goods.forEach((good) =>{
          good.foods.forEach((food) =>{
            if(food.count){
              foodList.push(food);
            }
          })
        })
        return foodList
      }
    },
    components:{
      shopcart:shopcart,
      addcontrol:addcontrol,
      food:food
      // chosefooods:chosefooods
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import './../common/common.scss';
  .goods{
    position: absolute;
    top:174px;
    left:0;
    bottom:48px;
    width:100%;
    display:flex;
    overflow:hidden;
    .menu-wrapper{
      flex:0 0 80px;
      width:80px;
      ul{
        background-color:rgba(7,17,27,0.1);
        .menu-item{
          box-sizing: border-box;
          display:table;
          height:54px;
          width:100%;
          padding:0 12px;
          &.current{
            background-color:#fff;
          }
          .text{
            position: relative;
            vertical-align:middle;
            display:table-cell;
            line-height:14px;
            font-size:12px;
            color:rgb(7,17,27);
            font-weight:200;
            @include after1px(rgba(7,17,27,0.1));
            &>.icon{
              display:inline-block;
              width:12px;
              height:12px;
              background-size:cover;
              &.decrease{
              @include bgimg('decrease_3');
              }
              &.discount{
                @include bgimg('discount_3');
              }
              &.special{
                @include bgimg('special_3');
              }
            }
          }
        }
      }
    }

    .con-wrapper{
      flex:1;
      // overflow:auto;
      ul{
        li{
          h2{
            box-sizing: border-box;
            height:26px;
            width:100%;
            padding-left:14px;
            line-height:26px;
            font-size:12px;
            color:rgb(147,153,159);
            border-left:5px solid #d9dde1;
            background-color:#f3f5f7;
          }
          ul{
            li{
              position: relative;
              padding:18px;
              @include after1px(rgba(7,17,27,0.1));
              &:last-child{
                @include after1px(white)
              }
              /* border-bottom:1px solid rgb(7,17,27); */
              img{
                width:57px;
                height:57px;
              }
              .information{
                position: relative;
                box-sizing: border-box;
                padding-left:10px;
                margin-left:67px;
                dt{
                  font-size:14px;
                  color:rgb(7,17,27);
                  line-height:14px;
                  margin-top:2px;
                }
                dl{
                  dd{
                    margin-top:8px;
                    font-size:10px;
                    color:rgb(147,153,159);
                    // line-height:10px;
                    span{
                      margin-right:12px;
                    }
                  }
                }
                p{
                  font-size:14px;
                  color:rgb(240,20,20);
                  font-weight:700;
                  line-height:48px;
                  del{
                    font-size:10px;
                    font-weight:700;
                    line-height:24px;
                    color:rgb(147,153,159);
                  }
                }
                // .addPrice{
                //   position: absolute;
                //   right: 0;
                //   bottom:0;
                //   span{
                //     font-size:24px;
                //     line-height:24px;
                //     color:rgb(0,160,220);
                //   }
                //   b{
                //     font-size:10px;
                //     color:rgb(147,153,159);
                //     line-height:24px;
                //   }
                // }
              }
            }
          }
        }
      }
    }

    .shopcart-wrapper{

    }

    .food-wrapper{
      // position: fixed;
      // top: 0;
      // left:0;
      // width:100%;
      // height:100%;
      // background-color:pink;
    }
  }

</style>