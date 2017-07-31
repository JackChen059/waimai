<template>
  <div class="boom">
    saybyebye
    {{boom[0].rateTime}}
  </div>
</template>

<script>
  export default{
    props:{
      boom:{
        return:Array
      }
    }
  }
</script>

<style lang="scss" scoped>
  .boom{
    font-size:30px;
  }
</style>