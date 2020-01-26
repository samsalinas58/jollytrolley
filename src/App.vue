<template>
  <div id="app">
    <Header />
    <TrolleyCard/>
    <Footer/>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.min.css'
import TrolleyCard from './components/TrolleyCard.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
Vue.use(VueMaterial)

export default {
  name: 'app',
  components: {
    Header,
    TrolleyCard,
    Footer
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;
}
</style>
