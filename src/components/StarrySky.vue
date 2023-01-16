<script setup lang="ts">
const props = defineProps<{
  width: number;
  height: number;
}>();

const maxView = Math.max(props.width, props.height);
const spread = Math.floor(maxView * 0.3);

function checkSuccess(rate: number): boolean {
  return Math.random() * 100 <= rate;
}

function randomColor(): string {
  let color = checkSuccess(50) ? "lightblue" : "lightpink";
  return checkSuccess(20) ? color : "white";
}

function randomSize(): number {
  let size = checkSuccess(50) ? 2 : 3;
  return checkSuccess(25) ? size : 1;
}

function getClass(): string {
  return checkSuccess(10) ? "twinkle star" : "star";
}

function getStyle(): Record<string, string> {
  return {
    width: `${randomSize()}px`,
    "background-color": randomColor(),
    top: `${Math.floor(Math.random() * maxView)}px`,
    left: `${Math.floor(Math.random() * maxView)}px`,
    "animation-delay": `${Math.floor(Math.random() * 1000)}ms`,
    "animation-duration": `${Math.floor(Math.random() * 10)}s`,
  };
}

function getDimensions(): Record<string, string> {
  return {
    height: `${props.height}px`,
    width: `${props.width}px`,
  };
}
</script>

<template>
  <div id="container" :style="getDimensions()">
    <div id="sky">
      <template v-for="n in spread">
        <div :class="getClass()" :style="getStyle()"></div>
      </template>
    </div>
  </div>
</template>

<style scoped>
#container {
  overflow: hidden;
  position: fixed;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
  border-radius: 50%;
}

#sky {
  width: inherit;
  height: inherit;
  position: relative;
  background-color: inherit;
  transform-origin: center;
  /* animation: rotate 1200s infinite; */
  z-index: -1;

  @media (prefers-reduced-motion: reduce) {
    animation: none;
  }
}

@keyframes rotate {
  to {
    transform: rotate(-360deg);
  }
}

.star {
  aspect-ratio: 1 / 1;
  position: absolute;
  border-radius: 50%;
}

.twinkle {
  animation: twinkle 1s ease-in-out alternate infinite;
  box-shadow: 0 0 0.5rem 0.05rem hsl(0, 0%, 90%);
}

@keyframes twinkle {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
