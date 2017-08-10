<template>
    <div class="addPrice">
  <transition name="move">
    <span class="less icon-remove_circle_outline" v-if="this.food.count" @click.stop.prevent="move($event)"></span>
  </transition>
    <b v-show="this.food.count">{{this.food.count}}</b>
    <span class="icon-add_circle add" @click.stop.prevent="show($event)"></span>
  </div>

</template>

<script>
import Vue from "vue"
  export default{
    methods:{
      show(event){
        if(!event._constructed){
          return
        }
        if(!this.food.count){
          Vue.set(this.food,'count',1)
        }else{
          this.food.count++
        }
        console.log(this.food.count);
      },
      move(event){
        if(!event._constructed){
          return
        }
        this.food.count--
      }
    },
    props:{
      food:{
        return:Object
      }
    },
    computed:{

    }
  }
</script>

<style lang="scss" scoped>
  .addPrice{
    position: absolute;
    right: 0;
    bottom:0;
    font-size:0;
    span{
      font-size:24px;
      line-height:24px;
      color:rgb(0,160,220);
    }
    b{
      display:inline-block;
      vertical-align:top;
      font-size:10px;
      padding-right: 6px;
      padding-left: 6px;
      color:rgb(147,153,159);
      line-height:24px;
    }
  }
  .move-enter-active, .move-leave-active{
    transition: all 0.5s linear;
    transform: translate3D(0,0,0) rotate(0deg);
  }
  .move-enter,.move-leave-active{
    transform: translate3D(24px,0,0) rotate(90deg);
    opacity:0;
  }
</style>