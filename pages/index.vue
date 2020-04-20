<template>
  <div class="container">
    <logo />
    <h1 class="title mt-4">breathe</h1>
    <h2 class="subtitle">Feel better, every breathe matters</h2>
    <div>
      <breathe />
    </div>
  </div>
</template>

<script>
import Breathe from '~/components/Breathe.vue'
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Breathe,
    Logo
  }
}
</script>

<style>
.container {
  @apply min-h-screen;
  @apply flex;
  @apply flex-col;
  @apply justify-center;
  @apply items-center;
  @apply text-center;
  @apply mx-auto;
  animation: 1s appear;
}

.title {
  @apply block;
  @apply font-light;
  @apply text-6xl;
  @apply text-gray-800;
  @apply leading-none;
}

.subtitle {
  @apply font-light;
  @apply text-xl;
  @apply text-gray-700;
}
</style>
