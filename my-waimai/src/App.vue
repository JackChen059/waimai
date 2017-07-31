<template>
  <div id="app">
  <iheader :seller="seller"></iheader>
    <div class="tab after1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>

    <router-view></router-view>
  </div>
</template>

<script>
import iheader from"./components/header/header.vue";

export default {
  name: 'app',
  data() {
    return{
      seller:Object
    };
  },
  created() {
     this.$http.get('api/seller').then(response => {
      this.seller = response.body.seller;
     })
    },
  components: {
    iheader:iheader
  }
}
</script>


<style scoped lang="scss">
@import'./components/common/common.scss';
.tab{
  height:40px;
  width:100%;
  display: flex;
  position: relative;
  // border-bottom:1px solid rgba(7,17,27,0.1);
  @include after1px(rgba(7,17,27,0.1))
  .tab-item{
    flex:1;
    text-align:center;
    &>a{
      height:100%;
      line-height:40px;
      color:rgb(77,85,93);
      display: block;
      font-size:14px;
      &.active{
        color:rgb(240,20,20);
      }
    }
  }
}
</style>
