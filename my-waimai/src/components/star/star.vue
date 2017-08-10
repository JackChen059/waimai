<template>
  <span class="star" :class="typesize">
    <ul>
      <li v-for="starCla in starNum" :class="starCla" class="star-item"></li>
    </ul>
  </span>
</template>

<script>
  export default {
    props:{
      star:{
        return: Number
      },
      size:{
        return:String
      }
    },

    computed:{
      typesize(){
        return 'type'+this.size
      },
      starNum(){
        let result = [];
        let star = Math.floor(this.star * 2 ) / 2;
        let hasDecimal = star % 1 !== 0;
        let integer = Math.floor(star);
        for(let i=0;i<integer;i++){
          result.push("on");
        }
        if(hasDecimal){
          result.push("half");
        }
        while(result.length < 5){
           result.push("off");
        }
        return result;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import './../common/common.scss';
  .star{
    font-size: 0;
    ul{
      text-align:center;
      .star-item{
        display: inline-block;
        background-repeat: no-repeat;
        width: 20px;
        height: 20px;
        margin-right: 22px;
        background-size: 100%;
      }
    }
      &.type24{
        .star-item{
          &:last-child{
            margin-right: 0;
          }
          &.on{
             @include bgimg(star24_on);
          }
          &.half{
            @include bgimg(star24_half);
          }
          &.off{
           @include bgimg(star24_off);
          }
        }
      }

      &.type36{
        .star-item{
          margin-right:6px;
          width:16px;
          height:16px;
          &:last-child{
            margin-right: 0;
          }
          &.on{
             @include bgimg(star36_on);
          }
          &.half{
            @include bgimg(star36_half);
          }
          &.off{
           @include bgimg(star36_off);
          }
        }
      }

      &.type48{
        .star-item{
          margin-right:10px;
          width:10px;
          height:10px;
          &:last-child{
            margin-right: 0;
          }
          &.on{
             @include bgimg(star48_on);
          }
          &.half{
            @include bgimg(star48_half);
          }
          &.off{
           @include bgimg(star48_off);
          }
        }
      }
  }

</style>
 


           


