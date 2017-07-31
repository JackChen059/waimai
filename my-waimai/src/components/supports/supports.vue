<template>
  <div class="supports clearfix" v-bind:class="[className]">
    <ul class="pull-left">
      <li v-for="(item,index) in supports">
        <span class="discou pull-left" :class="classMap[index]"></span>
      {{item.description}}
      </li>
    </ul>
    <!-- <div class="count-wrapper" @click="showDetail">
      <span class="pull-right numLi">{{classMap.length}}个<i class="icon-keyboard_arrow_right"></i></span>
    </div> -->
  </div>
</template>

<script>
  export default {
    props:{
      supports:{
        return:Array
      },
      className:{
        return:String
      }
    },
    data(){
      return{
        detailShow: false,
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
    }
  }
</script>

<style lang="scss" scoped>
  @import './../common/common.scss';
  .supports{
    font-size:10px;
    color:rgb(255,255,255);
    font-weight:200;
    line-height: 12px;
    li{
      // padding-bottom: 12px;
      .discou{
        display:inline-block;
        margin-right:4px;
        height:12px;
        width:12px;
        background-size:cover;
        &.decrease{
          @include bgimg('decrease_1');
        }
        &.discount{
          @include bgimg('discount_1');
        }
        &.guarantee{
          @include bgimg('guarantee_1');
        }
        &.invoice{
          @include bgimg('invoice_1');
        }
        &.special{
          @include bgimg('special_1');
        }
      }
    }


    &.bigSup{
        padding-top:10px;
        padding-bottom:28px;
      li{
        margin-bottom: 12px;
        &:last-chid(){
          margin-bottom:0;
        }
        .discou{
          display:inline-block;
          height:12px;
          width:12px;
          margin-right:6px;
          background-size:cover;
          &.decrease{
            @include bgimg('decrease_2');
          }
          &.discount{
            @include bgimg('discount_2');
          }
          &.guarantee{
            @include bgimg('guarantee_2');
          }
          &.invoice{
            @include bgimg('invoice_2');
          }
          &.special{
            @include bgimg('special_2');
          }
        }
      }
    }
  }
</style>