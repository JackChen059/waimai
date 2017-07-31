<template>
  <div class="ratings">
    <header>
      我是ratings
      <boom :boom="ratings.data" v-if="ratings.data"></boom>
    </header>
  </div>
</template>

<script>
import boom from "../boom/boom.vue";
export default {
  data(){
    return{
      ratings:Object
    }
  },
  created(){
    this.$http.get('api/ratings').then(response => {
      this.ratings = response.body
      // console.log(this.ratings);
    })
  },
  components:{
    boom:boom
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">


</style>