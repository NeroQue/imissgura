<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import guraImage from '@/assets/gura.png'

// Set the date/time when Gura left (example: 2025-06-01T12:00:00Z)
const goneSince = new Date('2025-04-30T15:00:00Z') // 2025-05-01T00:00:00+09:00 JST
const timer = ref('')
let intervalId = null

function updateTimer() {
  const now = new Date()
  let diff = Math.floor((now - goneSince) / 1000)
  const days = Math.floor(diff / (24 * 3600))
  diff %= 24 * 3600
  const hours = Math.floor(diff / 3600)
  diff %= 3600
  const minutes = Math.floor(diff / 60)
  const seconds = diff % 60
  timer.value = `${days} <b>days</b> ${hours} <b>hours</b> ${minutes} <b>minutes</b> ${seconds} <b>seconds</b>`
}

onMounted(() => {
  updateTimer()
  intervalId = setInterval(updateTimer, 1000)
})
onUnmounted(() => {
  clearInterval(intervalId)
})
</script>

<template>
  <div class="container">
    <img :src="guraImage" alt="Gura Placeholder" class="gura-img" />
    <h1>WE'LL MISS YOU GURA!!</h1>
    <p class="timer">It's been <span v-html="timer"></span> since you've been gone</p>
    <footer>
      <small>
        made by NeroQue<br />
        Very heavily "inspired" by <a href="https://imissfauna.com" target="_blank">imissfauna.com</a>
      </small>
    </footer>
  </div>
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
