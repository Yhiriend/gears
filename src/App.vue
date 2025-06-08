<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue'
import Gear from './components/Gear.vue'

const rotation = ref(0)
let frameId: number

const proximidad = ref(60) // valor inicial, rango 0 a 120

const rotacionInicial1 = ref(30)
const rotacionInicial2 = ref(134)
const rotacionInicial3 = ref(30)
const animando = ref(false)

const center = { x: 200, y: 200 }
const angles = [225, 98, 370] // grados para los vértices del triángulo
const sizes = [160, 120, 100]

const pos = computed(() => {
  // proximidad: 0 = centro, 120 = borde
  const r = proximidad.value
  return angles.map((deg, i) => {
    const rad = deg * Math.PI / 180
    const x = center.x + r * Math.cos(rad) - sizes[i] / 2
    const y = center.y + r * Math.sin(rad) - sizes[i] / 2
    return { left: `${x}px`, top: `${y}px` }
  })
})

const animate = () => {
  if (animando.value) {
    rotation.value += 0.3
    frameId = requestAnimationFrame(animate)
  }
}

const play = () => {
  if (!animando.value) {
    animando.value = true
    frameId = requestAnimationFrame(animate)
  }
}

onMounted(() => {
  frameId = requestAnimationFrame(animate)
})
onUnmounted(() => {
  cancelAnimationFrame(frameId)
})
</script>

<template>
  <div style="width: 400px; margin: 0 auto;">
    <label style="display: block; margin-bottom: 8px;">
      Proximidad al centro:
      <input type="range" min="0" max="120" v-model="proximidad" />
      {{ proximidad }}
    </label>
    <div style="display: flex; gap: 10px; margin-bottom: 10px;">
      <label>Rotación inicial Gear 1:
        <input type="number" v-model.number="rotacionInicial1" min="0" max="360" style="width: 60px;" />°
      </label>
      <label>Rotación inicial Gear 2:
        <input type="number" v-model.number="rotacionInicial2" min="0" max="360" style="width: 60px;" />°
      </label>
      <label>Rotación inicial Gear 3:
        <input type="number" v-model.number="rotacionInicial3" min="0" max="360" style="width: 60px;" />°
      </label>
      <button @click="play" :disabled="animando">Play</button>
    </div>
    <div style="position: relative; width: 400px; height: 400px;">
      <!-- Engranaje grande -->
      <Gear :size="160" :line1Angle="6.2" :line2Angle="0.75"
        :rotation="rotation + rotacionInicial1"
        :labels="['A','B']"
        :style="{position: 'absolute', ...pos[0]}" />
      <!-- Engranaje mediano (gira en sentido contrario) -->
      <Gear :size="120" :line1Angle="2.05" :line2Angle="3.2"
        :rotation="-rotation + rotacionInicial2"
        :labels="['C','D']"
        :style="{position: 'absolute', ...pos[1]}" />
      <!-- Engranaje pequeño -->
      <Gear :size="90" :line1Angle="3.1" :line2Angle="1.9"
        :rotation="rotation + rotacionInicial3"
        :labels="['E','F']"
        :style="{position: 'absolute', ...pos[2]}" />
      
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
