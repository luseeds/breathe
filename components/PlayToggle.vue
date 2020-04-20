<template>
  <button
    class="absolute"
    :class="{ 'play-button': !isPlaying, 'stop-button': isPlaying }"
    :aria-label="isPlaying ? 'Stop' : 'Start'"
    :title="isPlaying ? 'Stop' : 'Start'"
  ></button>
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

  height: 50px;

  border-color: transparent transparent transparent rgb(144, 205, 244);
  border-width: 25px 0 25px 50px;
  transition: 200ms border-width ease;
  border-style: solid;
}

.play-button:hover,
.stop-button:hover {
  border-color: transparent transparent transparent rgb(190, 227, 248);
}
.play-button {
  animation: pulse 2s infinite;
}

.stop-button {
  height: 40px;
  border-width: 0 0 0 40px;
  transform: translate(-50%, -50%);
}

@keyframes pulse {
  0% {
    transform: translate(-35%, -50%) scale(1);
  }
  50% {
    transform: translate(-35%, -50%) scale(1.2);
  }
  100% {
    transform: translate(-35%, -50%) scale(1);
  }
}
</style>
