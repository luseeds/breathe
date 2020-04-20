<template>
  <div class="breathe mt-8">
    <div class="relative">
      <viewer
        v-if="config"
        :arcs="arcs"
        :diff="diff"
        :action="currentStep && currentStep.action"
      />
      <play-toggle :is-playing="isPlaying" @click.native="togglePlay" />
    </div>
    <div
      class="text-xl text-gray-700 my-16"
      :class="{ 'opacity-0': !isPlaying }"
    >
      <span class="capitalize">{{ currentStep && currentStep.action }}</span>
      for
      <span class="bg-blue-300 text-blue-800 p-2 px-3 rounded">{{
        remainingStepSeconds
      }}</span>
      seconds
      <!-- Turns {{ turns }} -->
    </div>
    <div class="border-t-2 border-gray-200 pt-4">
      <button class="text-gray-700" @click="isConfigVisible = !isConfigVisible">
        {{ isConfigVisible ? 'Hide configuration' : 'Configure' }}
      </button>
    </div>
    <configuration
      v-show="isConfigVisible"
      :current-step="stepIndex"
      @config-updated="configUpdated"
    />
  </div>
</template>
<script>
import Configuration from '~/components/Configuration.vue'
import Viewer from '~/components/Viewer.vue'
import PlayToggle from '~/components/PlayToggle.vue'

export default {
  components: {
    Configuration,
    PlayToggle,
    Viewer
  },
  data() {
    return {
      isPlaying: false,
      isConfigVisible: false,
      stepIndex: 0,
      diff: 0,
      turns: 0,
      config: null,
      remainingStepSeconds: 0
    }
  },
  computed: {
    configuration() {
      return this.config?.filter((step) => step.duration)
    },
    currentStep() {
      return this.configuration?.[this.stepIndex] || this.configuration?.[0]
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
  },
  methods: {
    togglePlay() {
      this.isPlaying = !this.isPlaying
      requestAnimationFrame(this.update)
      this.reset()
    },
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
        if (this.remainingStepSeconds <= 0) {
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

      this.isPlaying && requestAnimationFrame(this.update)
    },
    reset() {
      this.lastStepTime = null
      this.secondTick = null
      this.secondStepCount = 0
      this.timePreviousSteps = 0
      this.stepIndex = 0
      this.turns = 0
      this.remainingStepSeconds = this.currentStep.duration
    },
    configUpdated(config) {
      this.config = config
      this.reset()
      // Hacky way to force a redraw
      // as the Viewer component watch diff for updating the canvas
      this.diff = this.diff ? 0 : 0.001
    }
  }
}
</script>
<style>
.breathe {
  animation: 2s appear;
}
</style>
