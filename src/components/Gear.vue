<template>
    <svg :width="size" :height="size" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg"
        :style="`transform: rotate(${rotation}deg); transition: transform 1s linear;`">
        <!-- Engranaje -->
        <g stroke="#2986F5" stroke-width="4">
            <circle cx="50" cy="50" r="28" fill="#B3D6FF" />
            <circle cx="50" cy="50" r="16" fill="white" />
            <!-- Dientes del engranaje (redondeados) -->
            <g>
                <path v-for="n in 8" :key="n" 
                    :d="`M ${50 + 28 * Math.cos((n - 1) * Math.PI / 4 - 0.2)} ${50 + 28 * Math.sin((n - 1) * Math.PI / 4 - 0.2)}
                         L ${50 + 36 * Math.cos((n - 1) * Math.PI / 4 - 0.1)} ${50 + 36 * Math.sin((n - 1) * Math.PI / 4 - 0.1)}
                         A 2 2 0 0 1 ${50 + 36 * Math.cos((n - 1) * Math.PI / 4 + 0.1)} ${50 + 36 * Math.sin((n - 1) * Math.PI / 4 + 0.1)}
                         L ${50 + 28 * Math.cos((n - 1) * Math.PI / 4 + 0.2)} ${50 + 28 * Math.sin((n - 1) * Math.PI / 4 + 0.2)}`"
                    fill="#B3D6FF" stroke="#2986F5" />
            </g>
        </g>
        <!-- Líneas internas -->
        <line :x1="50" :y1="50" :x2="x1" :y2="y1" stroke="#F55C47" stroke-width="2" />
        <line :x1="50" :y1="50" :x2="x2" :y2="y2" stroke="#F55C47" stroke-width="2" />
        <!-- Puntos verdes y letras -->
        <g>
            <circle :cx="x1" :cy="y1" r="4" fill="#2ecc40" />
            <text :x="x1" :y="y1+1.5" text-anchor="middle" font-size="6" fill="#fff" font-weight="bold">{{ labels[0] }}</text>
            <circle :cx="x2" :cy="y2" r="4" fill="#2ecc40" />
            <text :x="x2" :y="y2+1.5" text-anchor="middle" font-size="6" fill="#fff" font-weight="bold">{{ labels[1] }}</text>
        </g>
    </svg>
</template>

<script setup lang="ts">
const props = defineProps({
    size: { type: Number, default: 120 },
    line1Angle: { type: Number, default: 0 },
    line2Angle: { type: Number, default: Math.PI / 3 },
    rotation: { type: Number, default: 0 },
    labels: { type: Array, default: () => ["A", "B"] },
})

const x1 = 50 + 28 * Math.cos(props.line1Angle)
const y1 = 50 + 28 * Math.sin(props.line1Angle)
const x2 = 50 + 28 * Math.cos(props.line2Angle)
const y2 = 50 + 28 * Math.sin(props.line2Angle)
</script>