<template>
  <div class="supports clearfix" v-bind:class="[className]">
    <ul class="pull-left">
      <li v-for="(item,index) in supports">
        <span class="discou pull-left" :class="classMap[index]"></span>
        <span class="word">{{item.description}}</span>
      </li>
    </ul>
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

    &.SbigSup{
      margin:0 16px;
      ul{
        width:100%;
      }
      li{
        display:block;
        box-sizing: border-box;
        padding:16px 12px;
        border-top:1px solid rgba(7,17,27,0.1);
        .discou{
          display:inline-block;
          height:16px;
          width:16px;
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
        .word{
          font-size:12px;
          font-weight:200;
          color:rgb(7,17,27);
          line-height:16px;
        }
      }
    }
  }
</style>