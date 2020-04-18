<template>
  <div>
    <form class="flex">
      <label v-for="step in steps" :key="step.key" class="flex flex-col m-4">
        <input
          v-model.number="step.duration"
          type="number"
          class="border-teal-400
        border-2 text-center"
          :class="{ 'border-pink-600': step.key === currentStep }"
        />
        {{ step.action }}
      </label>
    </form>
  </div>
</template>
<script>
export default {
  props: {
    config: {
      type: Array,
      default: () => []
    },
    currentStep: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      steps: this.config
    }
  }
}
</script>
