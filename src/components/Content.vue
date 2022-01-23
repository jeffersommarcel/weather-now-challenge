<template>
  <div class="content">
    <Weather 
      city='nuuk' 
      country='GL'
    />
    <Weather 
      city='urubici'
      country='BR'
    />
    <Weather 
      city='nairobi'
      country='KE'
    />
  </div>
</template>

<script>
  import Weather from '@/components/Weather.vue'

  export default {
    components: {
      Weather
    },
    created() {
    }
  }
</script>

<style lang="scss" scoped>
  @import "./src/assets/scss/_variables.scss";

  .content {
    background:$app-background;
    max-width: 1120px;
    margin: 0 auto;

    height: 90vh;
    overflow: auto;

    display:flex;
    align-items: center;
    justify-content: center;

    
    @media screen and (max-width: 800px) {

      display:grid;
      grid-template-columns: 1fr;
      padding:0;
      margin: auto;
      place-items:center;

      .card{
        width:60%;
        margin-top:1rem;

        &:not(:first-child):not(:last-child){
          transform: scale(1);
        }
      }
    }
  }
</style>