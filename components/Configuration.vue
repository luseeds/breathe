<template>
  <div class="border-t-2 border-gray-200 mt-4">
    <form class="mx-auto mt-4 w-1/2">
      <div
        v-for="(step, i) in steps"
        :key="step.key"
        class="md:flex md:flex-wrap md:items-center mb-8"
      >
        <div class="md:w-1/2">
          <label
            class="block text-gray-700 font-bold md:text-right mb-1 md:mb-0 md:pr-4"
            :class="{ 'text-blue-500': step.key === currentStep }"
            :for="`step-${step.key}`"
          >
            {{ step.action }}
          </label>
        </div>
        <div class="md:w-1/2 relative">
          <input
            :id="`step-${step.key}`"
            v-model.number="step.duration"
            class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-2/3 py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500"
            type="number"
          />
          <div
            v-if="i !== steps.length - 1"
            class="text-gray-700 absolute inset-x-0"
          >
            &darr;
          </div>
        </div>
      </div>
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
