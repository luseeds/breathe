<template>
  <div class="container">
    <div>
      <!-- <logo /> -->
      <h1 class="title">breathe</h1>
      <h2 class="subtitle">Feel better, every breathe matters</h2>
      <div>
        <Breathe />
      </div>
    </div>
  </div>
</template>

<script>
import Breathe from '~/components/Breathe.vue'
// import Logo from '~/components/Logo.vue'

export default {
  components: {
    Breathe
    // Logo
  }
}
</script>

<style>
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
  animation: 1s appear;
}

.title {
  @apply block font-light text-6xl text-gray-800 leading-none;
}

.subtitle {
  @apply font-light text-xl text-gray-700;
}
</style>
