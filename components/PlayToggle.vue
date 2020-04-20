<template>
  <button
    class="cursor-pointer absolute"
    :class="{ 'play-button': !isPlaying, 'stop-button': isPlaying }"
    :aria-label="isPlaying ? 'Stop' : 'Start'"
    :title="isPlaying ? 'Stop' : 'Start'"
  >
    <svg
      v-if="!isPlaying"
      viewBox="0 0 512 512"
      xmlns="http://www.w3.org/2000/svg"
      width="75"
      class="fill-current text-blue-300 hover:text-blue-200"
    >
      <polygon points="">
        <animate
          attributeType="XML"
          attributeName="points"
          dur="2s"
          repeatCount="indefinite"
          values="0,0 512,256 0,512;
                    20,20 482,226 20,482;
                    0,0 512,256 0,512"
          keyTimes="0; 0.5; 1"
          calcMode="spline"
          keySplines=".5 0 .5 1; .5 0 .5 1"
        />
      </polygon>
    </svg>
    <svg
      v-if="isPlaying"
      viewBox="0 0 512 512"
      xmlns="http://www.w3.org/2000/svg"
      width="50"
      class="fill-current text-blue-300 hover:text-blue-200"
    >
      <polygon points="0,0 0,512 512,512 512,0" />
    </svg>
  </button>
</template>
<script>
export default {
  props: {
    isPlaying: {
      type: Boolean,
      default: false
    }
  }
}
</script>
<style>
.play-button,
.stop-button {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.play-button {
  transform: translate(-35%, -50%);
}
</style>
