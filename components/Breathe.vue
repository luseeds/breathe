<template>
  <div class="breathe">
    <Viewer :arcs="arcs" :diff="diff" />
    <div class="text-2xl font-bold my-16">
      <span class="capitalize">{{ currentStep.action }}</span> for
      <span class="bg-teal-500 text-white p-4 py-2">{{
        currentStep.duration
      }}</span>
      seconds
    </div>
    <Configuration :config="config" :current-step="step" />
  </div>
</template>
<script>
import Configuration from '~/components/Configuration.vue'
import Viewer from '~/components/Viewer.vue'

const BREATHE_PRESETS = {
  default: [
    { action: 'inhale', duration: 4, key: 0 },
    { action: 'hold', duration: 7, key: 1 },
    { action: 'exhale', duration: 8, key: 2 },
    { action: 'hold', duration: 0, key: 3 }
  ]
}

export default {
  components: {
    Configuration,
    Viewer
  },
  data() {
    return {
      step: 0,
      diff: 0,
      lastDraw: 0,
      config: [...BREATHE_PRESETS.default]
    }
  },
  computed: {
    currentStep() {
      return this.config?.[this.step]
    },
    arcs() {
      // do logic of converting config into drawable arcs
      const totalLength = this.config?.reduce(
        (total, step) => (step.duration || 0) + total,
        0
      )

      const slices = this.config
        ?.map((step) => ({
          value: (step.duration / totalLength) * 100,
          action: step.action
        }))
        .map((step) => ({ value: (2 / 100) * step.value, action: step.action })) // 2% of the slice (because the circle is 2)

      slices[0].start = 1.5
      slices[0].end = 1.5 + slices[0].value
      for (let i = 1; i < slices.length; i++) {
        slices[i].start = slices[i - 1].end
        slices[i].end = slices[i - 1].end + slices[i].value
      }

      return slices
    }
  },
  mounted() {
    this.start = new Date().getTime()
    this.lastDraw = this.start
    this.update()
  },
  methods: {
    update() {
      const time = new Date().getTime()
      const diff = time - this.start
      this.diff = time - this.lastDraw
      this.lastDraw = time
      let step = this.step
      if (diff > this.currentStep.duration * 1000) {
        step++
        this.start = new Date().getTime()
        // this.lastDraw = diff
      }

      if (step > this.config.length - 1) {
        step = 0
      }
      if (this.step !== step) {
        this.step = step
      }
      requestAnimationFrame(this.update)
    }
  }
}
</script>
<style>
.breathe {
  animation: 3s appear;
  margin: auto;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
