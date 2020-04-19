<template>
  <div class="breathe mt-8">
    <Viewer :arcs="arcs" :diff="diff" />
    <div class="text-xl text-gray-700 my-16">
      <span class="capitalize">{{ currentStep.action }}</span> for
      <span class="bg-blue-300 text-blue-800 p-2 px-3 rounded">{{
        remainingStepSeconds
      }}</span>
      seconds
      <!-- Turns {{ turns }} -->
    </div>
    <button class="" @click="isConfigVisible = !isConfigVisible">
      {{ isConfigVisible ? 'Hide configuration' : 'Configure' }}
    </button>
    <Configuration
      v-if="isConfigVisible"
      :config="config"
      :current-step="stepIndex"
    />
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
      isConfigVisible: false,
      stepIndex: 0,
      diff: 0,
      turns: 0,
      config: [...BREATHE_PRESETS.default],
      remainingStepSeconds: BREATHE_PRESETS.default[0].duration
    }
  },
  computed: {
    configuration() {
      return this.config?.filter((step) => step.duration)
    },
    currentStep() {
      return this.configuration[this.stepIndex]
    },
    totalDuration() {
      return this.configuration.reduce(
        (total, step) => (step.duration || 0) + total,
        0
      )
    },
    arcs() {
      // Converts the config into drawable arcs
      return this.configuration?.reduce((arcs, step) => {
        const value = step.duration / (this.totalDuration / 2) // /2 is because the circle is 2PI
        const isFirstArc = arcs.length === 0
        const start = isFirstArc
          ? 1.5 // 1.5 * Math.PI so that the first arc starts at the top
          : arcs[arcs.length - 1].end // starts at the previous step's end
        arcs.push({ ...step, value, start, end: start + value })

        return arcs
      }, [])
    }
  },
  mounted() {
    this.secondStepCount = 0
    this.timePreviousSteps = 0
    requestAnimationFrame(this.update)
  },
  methods: {
    update(currentTime) {
      if (!this.lastStepTime) {
        this.lastStepTime = currentTime
      }
      if (!this.secondTick) {
        this.secondTick = currentTime
      }

      this.diff =
        (2 * Math.PI * (currentTime - this.lastStepTime)) /
          1000 /
          this.totalDuration +
        this.timePreviousSteps
      // 1 second elapsed
      if (currentTime > this.secondTick + 1000) {
        this.secondStepCount++

        this.remainingStepSeconds =
          this.currentStep.duration - this.secondStepCount
        if (this.remainingStepSeconds === 0) {
          this.secondStepCount = 0
          this.lastStepTime = 0
          const isLastStep = this.stepIndex === this.configuration.length - 1
          this.timePreviousSteps = isLastStep ? 0 : this.diff
          this.stepIndex = isLastStep ? 0 : this.stepIndex + 1
          isLastStep && this.turns++
          this.remainingStepSeconds = this.currentStep.duration
        }
        this.secondTick = currentTime
      }

      requestAnimationFrame(this.update)
    }
  }
}
</script>
<style>
.breathe {
  animation: 1s appear;
}
</style>
