<script setup lang="ts">
defineOptions({
  name: 'IndexPage',
})

function showInstallPromotion() {
  // 显示自定义的添加到主屏幕提示
  // 例如，可以使用一个模态框来提示用户
  // ...
  // 然后，如果用户选择添加到主屏幕
  window.promptEvent.prompt()
  // 等待用户响应
  window.promptEvent.userChoice.then((choiceResult) => {
    if (choiceResult.outcome === 'accepted') {
      console.log('用户已接受添加到主屏幕')
    }
    else {
      console.log('用户已拒绝添加到主屏幕')
    }
  })
}
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
