<template>
  <Banner />
  <div class="main-container">
    <h1>Monte o seu burgeur:</h1>
    <BurgerForm />
  </div>
</template>

<script>
  import Banner from '../components/Landing.vue'
  import BurgerForm from '../components/BurgerForm.vue'

  export default {
    name: 'Home',
    components: {
    Banner,
    BurgerForm,
}
  }
</script>

<style scoped>

</style>
