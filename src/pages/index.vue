<script setup lang="ts">
defineOptions({
  name: 'IndexPage',
})
const user = useUserStore()
const name = ref(user.savedName)

const router = useRouter()
function go() {
  if (name.value)
    router.push(`/hi/${encodeURIComponent(name.value)}`)
}

const { t } = useI18n()

// 注册 Service Worker
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then((registration) => {
      console.log('Service Worker 注册成功:', registration)
    })
    .catch((error) => {
      console.error('Service Worker 注册失败:', error)
    })
}

// 监听 beforeinstallprompt 事件
window.addEventListener('beforeinstallprompt', (event) => {
  event.preventDefault()
  // 存储事件，以便可以稍后显示提示
  const promptEvent = event
  const showInstallPromotion = () => {
    // 显示自定义的添加到主屏幕提示
    // 例如，可以使用一个模态框来提示用户
    // ...
    // 然后，如果用户选择添加到主屏幕
    promptEvent.prompt()
    // 等待用户响应
    promptEvent.userChoice.then((choiceResult) => {
      if (choiceResult.outcome === 'accepted') {
        console.log('用户已接受添加到主屏幕')
      }
      else {
        console.log('用户已拒绝添加到主屏幕')
      }
    })
  }

  // 你可以在合适的时机调用 showInstallPromotion 来显示提示
  // 例如，当用户与页面交互一定次数后
  showInstallPromotion()
})
</script>

<template>
  <div>
    <div text-4xl>
      <div i-carbon-campsite inline-block />
    </div>
    <p>
      <a rel="noreferrer" href="https://github.com/antfu/vitesse" target="_blank">
        Vitesse
      </a>
    </p>
    <p>
      <em text-sm opacity-75>{{ t('intro.desc') }}</em>
    </p>

    <div py-4 />

    <TheInput
      v-model="name"
      :placeholder="t('intro.whats-your-name')"
      autocomplete="false"
      @keydown.enter="go"
    />
    <label class="hidden" for="input">{{ t('intro.whats-your-name') }}</label>

    <div>
      <button
        m-3 text-sm btn
        :disabled="!name"
        @click="go"
      >
        {{ t('button.go') }}
      </button>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
