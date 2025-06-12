<script setup lang="ts">
import OneGear from './components/OneGear.vue'
import { ref } from 'vue'

const isPlaying = ref(false)
const rotationCount = ref(0)

const toggleAnimation = () => {
  isPlaying.value = !isPlaying.value
  if (!isPlaying.value) {
    rotationCount.value = 0
  }
}

const handleRotationComplete = (count: number) => {
  rotationCount.value = count
  if (count === 3) {
    isPlaying.value = false
  }
}
</script>

<template>
  <div class="main">
    <div class="wrapper">
      <OneGear size="normal" :teeth="6" :duration="3" :clockwise="true" :top="20" :left="47" :line1Angle="100" :line2Angle="138" :isPlaying="isPlaying" :rotationCount="rotationCount" />
      <OneGear size="small" :teeth="4" :duration="2" :top="125" :left="145" :line1Angle="315" :line2Angle="78" :isPlaying="isPlaying" :rotationCount="rotationCount" />
      <OneGear size="big" :teeth="8" :duration="4" :top="0" :left="220" :clockwise="true" :line1Angle="280" :line2Angle="258" :isPlaying="isPlaying" :rotationCount="rotationCount" @rotation-complete="handleRotationComplete" />
    </div>
    <button class="play-pause-btn" @click="toggleAnimation">
      {{ isPlaying ? '⏸️' : '▶️' }}
    </button>
  </div>
</template>

<style>
.main {
  min-height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #111;
  gap: 20px;
}

.wrapper {
  width: 460px;
  height: 240px;
  position: relative;
  background: transparent;
}

.play-pause-btn {
  background: none;
  border: none;
  font-size: 2rem;
  cursor: pointer;
  padding: 10px;
  border-radius: 50%;
  transition: transform 0.2s;
}

.play-pause-btn:hover {
  transform: scale(1.1);
}

.play-pause-btn:active {
  transform: scale(0.95);
}
</style>
