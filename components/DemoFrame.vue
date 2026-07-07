<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{
  src: string
  title?: string
}>()

const copied = ref(false)
const failed = ref(false)

async function copySource() {
  failed.value = false
  try {
    const res = await fetch(props.src)
    const html = await res.text()
    await navigator.clipboard.writeText(html)
    copied.value = true
    setTimeout(() => (copied.value = false), 2000)
  } catch (e) {
    console.error('Copy source failed', e)
    failed.value = true
    setTimeout(() => (failed.value = false), 2000)
  }
}
</script>

<template>
  <div class="demo-frame">
    <iframe
      :src="src"
      :title="title || 'Demo'"
      class="demo-frame__iframe"
      loading="lazy"
    />
    <button
      class="demo-frame__btn"
      :title="failed ? 'Copy failed' : copied ? 'Copied!' : 'Copy source code'"
      @click="copySource"
    >
      <svg v-if="copied" width="18" height="18" viewBox="0 0 24 24" fill="none"
        stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="20 6 9 17 4 12" />
      </svg>
      <svg v-else width="18" height="18" viewBox="0 0 24 24" fill="none"
        stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <rect x="9" y="9" width="13" height="13" rx="2" ry="2" />
        <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1" />
      </svg>
    </button>
  </div>
</template>

<style scoped>
.demo-frame {
  position: relative;
  height: calc(100% - 3rem);
}
.demo-frame__iframe {
  width: 100%;
  height: 100%;
  border: 1px solid rgba(107, 114, 128, 0.5);
  border-radius: 0.5rem;
}
.demo-frame__btn {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  z-index: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.35rem;
  border-radius: 0.375rem;
  border: 1px solid rgba(107, 114, 128, 0.5);
  color: #374151;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(2px);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.15);
  cursor: pointer;
  transition: background 0.15s ease;
}
.demo-frame__btn:hover {
  background: rgba(255, 255, 255, 1);
}
</style>
