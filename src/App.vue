<template>
  <div class="bg-gray-900 text-white min-h-screen">
    <Navbar @navigate="navigate" />
    <div class="container mx-auto p-4">
      <component :is="currentComponent" />
    </div>
  </div>
</template>

<script lang="ts">
import Navbar from './components/Navbar.vue'
import Home from './views/Home.vue'
import Settings from './views/Settings.vue'

export default {
  components: {
    Navbar,
    Home,
    Settings
  },
  data() {
    return {
      currentComponent: 'Home'
    }
  },
  methods: {
    navigate(component: string) {
      this.currentComponent = component === 'home' ? 'Home' : 'Settings';
    }
  }
}
</script>

<style scoped>
</style>