<script setup lang="ts">
defineOptions({
  name: 'IndexPage',
})

const promptEvent = ref(null)

// 注册 Service Worker
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then((registration) => {
      console.log('Service Worker 注册成功:', registration)
    })
    .catch((error) => {
      console.error('Service Worker 注册失败:', error)
    })
}

function showInstallPromotion() {
  // 显示自定义的添加到主屏幕提示
  // 例如，可以使用一个模态框来提示用户
  // ...
  // 然后，如果用户选择添加到主屏幕
  promptEvent.value.prompt()
  // 等待用户响应
  promptEvent.value.userChoice.then((choiceResult) => {
    if (choiceResult.outcome === 'accepted') {
      console.log('用户已接受添加到主屏幕')
    }
    else {
      console.log('用户已拒绝添加到主屏幕')
    }
  })
}

// 监听 beforeinstallprompt 事件
window.addEventListener('beforeinstallprompt', (event) => {
  event.preventDefault()
  // 存储事件，以便可以稍后显示提示
  promptEvent.value = event
})
</script>

<template>
  <div>
    <div>
      <button
        m-3 text-sm btn
        @click="showInstallPromotion"
      >
        点击
      </button>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
