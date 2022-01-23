<template>
  <div class="loading" v-show="active">
    <img src="@/assets/images/loader.svg" alt="spinner">
  </div>
</template>

<script>
  export default {
    props: {
      active: Boolean
    }
  }
</script>

<style lang="scss" scoped>
  .loading{
    position:absolute;
    display:flex;
    align-items:center;
    justify-content:center;
    background:#fff;
    
    width: auto;
    height: auto;
    min-width: 100%;
    min-height: 100%;
    z-index: 99999;
    top:0;
    left:0;
  }
</style>