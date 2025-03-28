<template>
  <div>
    <Header :logo="logo_src" :alt="app_name" />
    <div>
      <router-view></router-view>
    </div>
  <Footer />
  </div>
</template>
<script>
import Header from './components/header/header.vue'
import Footer from './components/footer/footer.vue'

export default {
  data: function() {
    return {
      logo_src: "/img/logo.png",
      app_name: "Search"
    }
  },
  components: {
    Header,
    Footer
  }
}
</script>