<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

const props = defineProps({
  interval: {
    type: Number,
    default: 500,
  },
  // Override the image set; defaults to the curated map showcase.
  images: {
    type: Array as () => string[],
  },
})

const maps = props.images.map(name => `/assets/maps/${name}.jpg`)

const index = ref(0)
let timer: ReturnType<typeof setInterval> | undefined

onMounted(() => {
  timer = setInterval(() => {
    index.value = (index.value + 1) % maps.length
  }, props.interval)
})

onUnmounted(() => {
  if (timer) clearInterval(timer)
})
</script>

<template>
  <div class="map-carousel">
    <transition name="map-fade">
      <img
        :key="maps[index]"
        :src="maps[index]"
        class="map-carousel__img"
        alt="w3geo Karten-Showcase"
      />
    </transition>
  </div>
</template>

<style scoped>
.map-carousel {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: 0.75rem;
  background: #000;
}

.map-carousel__img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.map-fade-enter-active,
.map-fade-leave-active {
  transition: opacity 0.35s ease;
}

.map-fade-enter-from,
.map-fade-leave-to {
  opacity: 0;
}
</style>
