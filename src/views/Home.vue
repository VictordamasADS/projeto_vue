<template>
  <Banner />
  <div class="main-container">
    <h1 class="titulo">Escolha os seus Ingredientes</h1>
    <PizzaForm />
  </div>
</template>

<script>
  import Banner from "../components/Banner.vue"
  import PizzaForm from "../components/PizzaForm.vue"

  export default {
    name: "HOME",
    components: {
      Banner,
      PizzaForm
    }
  }
</script>

<style>
  .titulo {
    margin-right: 50px;
  }
</style>
