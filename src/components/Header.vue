<template>
  <header>
    <img src="@/assets/images/logo.svg" alt="weather now">
  </header>
</template>

<style lang="scss" scoped>
  @import "./src/assets/scss/_variables.scss";

  header{
    max-width: 1120px;
    margin: 0 auto;

    padding: 1rem;

    display: flex;
    align-items: center;
    justify-content: center;

    img{
      height: 1.5rem;
      width: auto;
    }
    
  }

</style>