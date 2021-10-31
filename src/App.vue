<template>
  <div class="app">
    <AppHeader/>
    <Management/>
    <AppFooter/>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import Management from './components/Management.vue'
import AppFooter from './components/AppFooter.vue'

export default {
  components: {
    AppHeader,
    Management,
    AppFooter
  }
}
</script>

<style>
  .app {
    height: 100%;
  }
</style>
