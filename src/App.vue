<script scope>
import { RouterLink, RouterView } from 'vue-router'
import NavbarVue from './components/Navbar.vue';
import FooterVue from './components/Footer.vue';

export default {
  components: {
    NavbarVue,
    FooterVue
  },
  data() {
    return {
      logo_src: "/image/logo.png",
      app_name: "Logo da aplicação"
    }
  }
}

</script>
<template>
  <header>
    <div>
      <nav>
        <NavbarVue :logo="logo_src" :alt="app_name" />
        <RouterView />
        <FooterVue />
      </nav>
    </div>
  </header>
</template>

<style scoped>
.main-container {
  margin: 50px;
  min-height: 250px;
}
</style>
