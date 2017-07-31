<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item">
          <span class="text">
            <span class="icon" v-if="item.type > 0" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>

    <div class="con-wrapper">
      <ul>
        <li v-for="item in goods">
          <h2>{{item.name}}</h2>
          <ul>
            <li v-for="food in item.foods" class="clearfix">
              <img :src="food.icon" class="pull-left">
              <div class="information">
                <dt>{{food.name}}</dt>
                <dl>
                  <dd>{{food.description}}</dd>
                  <dd><span>月售{{food.sellCount}}</span><span>好评率{{food.rating}}%</span></dd>
                </dl>
                <p>￥{{food.price}}</p>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
     data(){
      return{
        goods:Object
      }
    },
    created(){
      this.$http.get('api/goods').then(response => {
        this.goods = response.body.data
        this.classMap = ['decrease','discount','special']
      })
    },
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
      .menu-item{
        box-sizing: border-box;
        display:table;
        height:54px;
        width:100%;
        padding:0 12px;
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
    .con-wrapper{
      flex:1;
      overflow:auto;
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
              padding:18px;
              img{
                width:57px;
                height:57px;
              }
              .information{
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
                }
              }
            }
          }
        }
      }
    }
  }

</style>