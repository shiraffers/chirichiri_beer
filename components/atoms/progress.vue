<template>
  <div class="progress-bar-bg" v-if="isProcessing">
    <div class="progress-bar"></div>
  </div>
</template>

<script>
export default {
  computed: {
    isProcessing() {
      return true;
    }
  }
};
</script>
<style lang="scss">
$animation-time: 10s;

.progress-bar-bg {
  width: 100%;
  height: 3px;
  background: #eee;
  .progress-bar {
    overflow: no-display;
    position: absolute;
    left: 0;
    width: 0;
    height: 3px;
    background: #1496ed;
    background-position: 100px 100px;

    animation-name: moveIndeterminate;
    animation-duration: $animation-time;
    animation-timing-function: all $animation-time
      cubic-bezier(0.235, 0.285, 0.975, 0.055);
  }
}

@keyframes moveIndeterminate {
  0% {
    width: 0;
  }
  10% {
    width: 15%;
  }
  100% {
    width: 100%;
  }
}
</style>