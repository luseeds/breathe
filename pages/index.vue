<template>
  <div>
    <div class="container mt-2">
      <logo />
      <h1 class="title mt-8">breathe</h1>
      <h2 class="subtitle">Feel better, every breathe matters</h2>
      <div>
        <breathe />
      </div>
    </div>
    <div class="my-8 px-4 mx-auto max-w-xl text-justify text-gray-700">
      Our daily lives are full of a constant flow of information, thoughts,
      feelings. We feel like time is flying away and we're not in control. What
      if, for a short moment, we take control back. This is where Breathe comes
      in. Just settle down for a moment, relax and breathe! Be in the present
      moment, that's all that matters. Simply, breathe.
    </div>
    <footer
      class="footer border-t px-6 py-4 text-gray-700 bg-white text-center"
    >
      A
      <a href="https://github.com/luseeds/breathe">project</a>
      by
      <a href="https://luseeds.com" class="text-green-800 hover:text-green-500"
        >luseeds</a
      >
    </footer>
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
