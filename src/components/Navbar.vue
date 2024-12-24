<template>
  <nav class="bg-gray-800 p-4">
    <div class="container mx-auto flex justify-between">
      <div class="text-white text-lg">Photo App</div>
      <div>
        <button @click="showHome" class="text-white px-4">Home</button>
        <button @click="showSettings" class="text-white px-4">Settings</button>
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
export default {
  name: 'Navbar',
  methods: {
    showHome() {
      this.$emit('navigate', 'home');
    },
    showSettings() {
      this.$emit('navigate', 'settings');
    }
  }
}
</script>

<style scoped>
</style>