<template>
  <BannerVue />
  <main class="main-container">
    <h1>Monte o seu burger:</h1>
    <FormularioVue/>
  </main>
</template>
<script>
import BannerVue from '../components/Banner.vue';
import FormularioVue from '../components/Formulario.vue';

export default {
  name: "Home",
  components: {
    BannerVue,
    FormularioVue
  }
}
</script>
