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
    <div class="demo-frame__bar">
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
    <iframe
      :src="src"
      :title="title || 'Demo'"
      class="demo-frame__iframe"
      loading="lazy"
    />
  </div>
</template>

<style scoped>
.demo-frame {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  height: calc(100% - 3rem);
}
.demo-frame__bar {
  display: flex;
  justify-content: flex-end;
}
.demo-frame__btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.35rem;
  border-radius: 0.375rem;
  border: 1px solid rgba(107, 114, 128, 0.5);
  background: rgba(127, 127, 127, 0.12);
  cursor: pointer;
  transition: background 0.15s ease;
}
.demo-frame__btn:hover {
  background: rgba(127, 127, 127, 0.25);
}
.demo-frame__iframe {
  flex: 1;
  width: 100%;
  border: 1px solid rgba(107, 114, 128, 0.5);
  border-radius: 0.5rem;
}
</style>
