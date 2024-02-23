<script setup lang="ts">
import Versions from './components/Versions.vue'

const ipcHandle = async () => {
  window.electron.ipcRenderer.send('ping', 'ping-message')

  // innvoke
  const invokeReplyMessage = await window.electron.ipcRenderer.invoke('invoke', 'invoke_data')
  console.log('invokeReplyMessage:', invokeReplyMessage)

  const sendSyncReplyMessage = await window.electron.ipcRenderer.sendSync(
    'sendSync',
    'sendReplay_data'
  )
  console.log('sendSyncReplyMessage:', sendSyncReplyMessage)
}

//  event.reply('reply', 'main_data')
window.electron.ipcRenderer.on('reply', (event, message) => {
  console.log('replayEvent', event)
  console.log('replyMessage', message)
})

window.electron.ipcRenderer.on('messageToRenderer', (event, message) => {
  console.log('messageToRenderer:', event, message)
})
</script>

<template>
  <img alt="logo" class="logo" src="./assets/electron.svg" />
  <div class="creator">Powered by electron-vite</div>
  <div class="text">
    Build an Electron app with
    <span class="vue">Vue</span>
    and
    <span class="ts">TypeScript</span>
  </div>
  <p class="tip">Please try pressing <code>F12</code> to open the devTool</p>
  <div class="actions">
    <div class="action">
      <a href="https://electron-vite.org/" target="_blank" rel="noreferrer">Documentation</a>
    </div>
    <div class="action">
      <a target="_blank" rel="noreferrer" @click="ipcHandle">Send IPC</a>
    </div>
  </div>
  <Versions />
</template>
