<template>
  <div class="box">
    <button @click="handleClick">Click me for Notification</button>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import orangeIcon from '~/assets/image/orange.png'

const handleClick = () => {
  Notification.requestPermission().then((perm) => {
    if (perm === 'granted') {
      const notification = new Notification('example notification', {
        body: Math.random(),
        data: { hello: 'world' },
        icon: orangeIcon,
        // tag: 'welcome'
      })
      notification.addEventListener('error', (e) => {
        alert('error')
      })
    }
  })
}

let notification
let interval

const handleVisibilityChange = () => {
  if (document.visibilityState === 'hidden') {
    const leaveDate = new Date()
    interval = setInterval(() => {
      notification = new Notification('come back please', {
        body: `you have been gone for ${Math.round((new Date() - leaveDate) / 1000)} seconds`,
        tag: 'come back'
      })
    }, 1000)
  } else {
    if (interval) clearInterval(interval)
    if (notification) notification.close()
  }
}

onMounted(() => {
  document.addEventListener('visibilitychange', handleVisibilityChange)
})

onUnmounted(() => {
  document.removeEventListener('visibilitychange', handleVisibilityChange)
})
</script>
<style>
.box{
  width: 100%;
  height: 800px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
