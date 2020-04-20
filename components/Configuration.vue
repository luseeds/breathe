<template>
  <div class="border-t-2 border-gray-200 mt-4 pt-4">
    <div class="md:flex md:flex-wrap md:items-center mb-8">
      <div class="md:w-1/2">
        <label
          class="block text-gray-700 font-bold md:text-right mb-1 md:mb-0 md:pr-4"
          :for="`presets`"
        >
          Preset
        </label>
      </div>
      <div class="md:w-1/2 relative">
        <select
          id="presets"
          v-model="selectedPreset"
          class="block appearance-none w-full bg-gray-200 border-2 border-gray-200 text-gray-700 py-3 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-blue-500"
          @change="$emit('config-updated', steps)"
        >
          <option v-for="preset in presets" :key="preset.name" :value="preset">
            {{ preset.name }}
          </option>
        </select>
        <div
          class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
        >
          <svg
            class="fill-current h-4 w-4"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
          >
            <path
              d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
            />
          </svg>
        </div>
      </div>
    </div>

    <form class="mx-auto mt-4 w-1/2">
      <div
        v-for="(step, i) in steps"
        :key="step.key"
        class="md:flex md:flex-wrap md:items-center mb-8"
      >
        <div class="md:w-1/2">
          <label
            class="block text-gray-700 font-bold md:text-right mb-1 md:mb-0 md:pr-4 capitalize"
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
            disabled
            class="cursor-not-allowed bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full text-center py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500"
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
const PRESETS = [
  {
    name: 'Relaxing Breathe',
    selected: true,
    steps: [
      { action: 'inhale', duration: 4, key: 0 },
      { action: 'hold', duration: 7, key: 1 },
      { action: 'exhale', duration: 8, key: 2 },
      { action: 'hold', duration: 0, key: 3 }
    ]
  },
  {
    name: 'Letting go',
    steps: [
      { action: 'inhale', duration: 4, key: 0 },
      { action: 'hold', duration: 0, key: 1 },
      { action: 'exhale', duration: 6, key: 2 },
      { action: 'hold', duration: 0, key: 3 }
    ]
  },
  {
    name: 'Regular',
    steps: [
      { action: 'inhale', duration: 4, key: 0 },
      { action: 'hold', duration: 4, key: 1 },
      { action: 'exhale', duration: 4, key: 2 },
      { action: 'hold', duration: 4, key: 3 }
    ]
  },
  {
    name: 'Deep breathing',
    steps: [
      { action: 'inhale', duration: 7, key: 0 },
      { action: 'hold', duration: 0, key: 1 },
      { action: 'exhale', duration: 7, key: 2 },
      { action: 'hold', duration: 0, key: 3 }
    ]
  }
]

export default {
  props: {
    currentStep: {
      type: Number,
      default: 0
    }
  },
  data() {
    const presets = [...PRESETS]
    return {
      presets,
      selectedPreset: presets[0]
    }
  },
  computed: {
    steps() {
      return this.selectedPreset.steps
    }
  },
  mounted() {
    this.$emit('config-updated', [...this.steps])
  }
}
</script>
