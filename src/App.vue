<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { Analytics } from "@vercel/analytics/vue"

// Set the date/time when Gura left (example: 2025-06-01T12:00:00Z)
const goneSince: Date = new Date('2025-04-30T15:00:00Z') // 2025-05-01T00:00:00+09:00 JST
const timer = ref<string>('')
let intervalId: number | null = null

function updateTimer(): void {
  const now: Date = new Date()
  let diff: number = Math.floor((now.getTime() - goneSince.getTime()) / 1000)
  const days: number = Math.floor(diff / (24 * 3600))
  diff %= 24 * 3600
  const hours: number = Math.floor(diff / 3600)
  diff %= 3600
  const minutes: number = Math.floor(diff / 60)
  const seconds: number = diff % 60
  timer.value = `${days} <b>days</b> ${hours} <b>hours</b> ${minutes} <b>minutes</b> ${seconds} <b>seconds</b>`
}

onMounted(() => {
  updateTimer()
  intervalId = window.setInterval(updateTimer, 1000)
})

onUnmounted(() => {
  if (intervalId !== null) {
    window.clearInterval(intervalId)
  }
})
</script>

<template>
  <Analytics />
  <router-view />
</template>

<style scoped>
:global(html), :global(body) {
  height: 100%;
  margin: 0;
  padding: 0;
  background: #7fbbf3;
  overflow: hidden;
}
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: #111;
}
.gura-img {
  width: 350px;
  max-width: 80vw;
  margin-bottom: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.08);
}
h1 {
  font-size: 2.5rem;
  font-weight: bold;
  margin-bottom: 16px;
}
.timer {
  font-size: 1.2rem;
  margin-bottom: 40px;
}
footer {
  position: fixed;
  left: 0;
  bottom: 10px;
  width: 100vw;
  text-align: left;
  padding-left: 10px;
  color: #2196f3;
  font-size: 0.95rem;
}
footer a {
  color: #1565c0;
  text-decoration: underline;
}
</style>
