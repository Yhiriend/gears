<template>
  <div class="gear" :class="sizeClass" :style="positionStyle">
    <div class="gear-inner" :class="{ 'rotate-clockwise': clockwise, 'rotate-counter-clockwise': !clockwise, 'paused': !isPlaying }"
      :style="{ animationDuration: duration + 's' }">
      <div v-for="n in teeth" :key="n" class="bar" :style="{ transform: 'rotate(' + ((n - 1) * (360 / teeth)) + 'deg)' }">
      </div>
      <div class="center-circle"></div>
      <div class="red-line line1" :style="{ transform: 'rotate(' + line1Angle + 'deg)' }"></div>
      <div class="red-line line2" :style="{ transform: 'rotate(' + line2Angle + 'deg)' }"></div>
    </div>
    <div class="rotation-counter">
      Vueltas: {{ rotationCount }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, onMounted, onUnmounted } from 'vue'

type GearSize = 'big' | 'normal' | 'small'

interface Props {
  size?: GearSize
  teeth?: number
  duration?: number
  clockwise?: boolean
  top?: string | number
  left?: string | number
  right?: string | number
  bottom?: string | number
  line1Angle?: number
  line2Angle?: number
  isPlaying?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  size: 'small',
  teeth: 3,
  duration: 3,
  clockwise: false,
  top: 'auto',
  left: 'auto',
  right: 'auto',
  bottom: 'auto',
  line1Angle: 0,
  line2Angle: 180,
  isPlaying: true
})

const emit = defineEmits(['rotation-complete'])

const rotationCount = ref(0)
let animationStartTime: number | null = null
let lastRotationTime: number | null = null

const updateRotationCount = () => {
  if (!props.isPlaying) return

  const now = performance.now()
  if (!animationStartTime) {
    animationStartTime = now
    lastRotationTime = now
    return
  }

  const elapsedTime = now - lastRotationTime!
  const rotationDuration = props.duration * 1000

  if (elapsedTime >= rotationDuration) {
    rotationCount.value++
    lastRotationTime = now
    emit('rotation-complete', rotationCount.value)
  }
}

let animationFrameId: number

onMounted(() => {
  const animate = () => {
    updateRotationCount()
    animationFrameId = requestAnimationFrame(animate)
  }
  animate()
})

onUnmounted(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId)
  }
})

const sizeClass = computed(() => {
  switch (props.size) {
    case 'big':
      return 'big'
    case 'normal':
      return 'normal'
    default:
      return 'small'
  }
})

const positionStyle = computed(() => ({
  position: 'absolute',
  top: typeof props.top === 'number' ? props.top + 'px' : props.top,
  left: typeof props.left === 'number' ? props.left + 'px' : props.left,
  right: typeof props.right === 'number' ? props.right + 'px' : props.right,
  bottom: typeof props.bottom === 'number' ? props.bottom + 'px' : props.bottom
}))
</script>

<style scoped>
.gear {
  position: absolute;
  border-radius: 60px;
}

.gear.small {
  width: 60px;
  height: 60px;
}

.gear.small .center-circle {
  height: 36px;
  width: 36px;
  border-radius: 36px;
  margin-left: -18px;
  margin-top: -18px;
}

.gear.small .gear-inner .bar {
  margin-left: -38px;
  width: 76px;
}

.gear.small .red-line {
  height: 30px;
  margin-top: -15px;
}

.gear.normal {
  width: 120px;
  height: 120px;
}

.gear.normal .center-circle {
  height: 96px;
  width: 96px;
  border-radius: 48px;
  margin-left: -48px;
  margin-top: -48px;
}

.gear.normal .gear-inner .bar {
  margin-left: -68px;
  width: 136px;
}

.gear.normal .red-line {
  height: 60px;
  margin-top: -30px;
}

.gear.big {
  width: 240px;
  height: 240px;
}

.gear.big .center-circle {
  height: 192px;
  width: 192px;
  border-radius: 96px;
  margin-left: -96px;
  margin-top: -96px;
}

.gear.big .gear-inner .bar {
  margin-left: -136px;
  width: 272px;
}

.gear.big .red-line {
  height: 120px;
  margin-top: -60px;
}

.center-circle {
  position: absolute;
  content: '';
  background-color: #111;
  top: 50%;
  left: 50%;
  z-index: 3;
  box-shadow: 0 0 10px rgb(255, 255, 255, 0.1), inset 0 0 10px rgb(0, 0, 0, 0.1), inset 0 2px 0 0 #090909, inset 0 -1px 0 0 #3cf0ed;
}

.gear-inner {
  position: relative;
  height: 100%;
  width: 100%;
  background: #3cf0ed;
  border-radius: 30px;
  animation: rotate-counter-clockwise 3s linear infinite;
}

.gear-inner.paused {
  animation-play-state: paused;
}

.gear.normal .gear-inner {
  border-radius: 60px;
}

.gear.big .gear-inner {
  border-radius: 120px;
}

.red-line {
  position: absolute;
  width: 3px;
  background-color: #ff0000;
  top: 25%;
  left: 50%;
  transform-origin: bottom center;
  margin-left: -1.5px;
  z-index: 5;
  box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
  pointer-events: none;
}

.rotation-counter {
  position: absolute;
  top: -25px;
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 12px;
  white-space: nowrap;
  z-index: 6;
}

@keyframes rotate-counter-clockwise {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(-360deg);
  }
}

@keyframes rotate-clockwise {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}

.rotate-clockwise {
  animation-name: rotate-clockwise !important;
}

.rotate-counter-clockwise {
  animation-name: rotate-counter-clockwise !important;
}

.gear-inner .bar {
  position: absolute;
  background: #3cf0ed;
  height: 16px;
  left: 50%;
  top: 50%;
  margin-top: -8px;
  border-radius: 2px;
}
</style>