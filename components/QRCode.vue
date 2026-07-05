<script setup lang="ts">
import { renderSVG } from 'uqr'
import { computed, onMounted, ref } from 'vue'

// `value` is optional — when omitted, the QR code encodes the current page URL.
const props = defineProps<{ value?: string }>()

const currentUrl = ref('')
onMounted(() => {
  currentUrl.value = window.location.href
})

const target = computed(() => props.value ?? currentUrl.value)
const svg = computed(() => (target.value ? renderSVG(target.value) : ''))
</script>

<template>
  <div v-html="svg" />
</template>
