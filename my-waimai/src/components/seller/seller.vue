<template>
  <div class="seller">
    <header>
      我是seller
    </header>
  </div>
</template>

<script>
export default {
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
  components:{

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">


</style>