<script setup>
import { onMounted, reactive, ref } from 'vue'

const fromInformations = JSON.parse(localStorage.settings).from
const messages = JSON.parse(localStorage.settings).messages

const isTyping = ref(false)

const renderedMessages = reactive([])

const pushMessage = (message, index) => {
  setTimeout(() => {
    renderedMessages.push(message)
    if (message.type === 'from') {
      isTyping.value = false
    } else {
      isTyping.value = true
    }
  }, index * message.waitAfter)

}

onMounted(() => {
  for (let i = 0; i < messages.length; i++) {
    pushMessage(messages[i], i)
  }
  document.querySelector('input').focus()
})


</script>

<template>
  <input type="hidden">
  <div class="chat-header">
    <div class="flex items-center gap-2">
      <img v-if="fromInformations.profilePicture" :src="fromInformations.profilePicture">
      <h1>{{ fromInformations.name }}</h1>
    </div>
    <div class="typing" v-if="!isTyping">
      is online
    </div>
    <div class="typing" v-if="isTyping">
      is typing...
    </div>
  </div>
  <div class="chat-wrapper">
    <div class="chat chat-start" v-for="(message, index) in renderedMessages" :key="index"
      :class="{ 'chat-end': message.type === 'to' }">
      <div class="chat-bubble">{{ message.text }}</div>
    </div>

  </div>
</template>

<style scoped>
.chat-header {
  display: flex;
  align-items: center;
  gap: .2em;
  border: 1px solid hsl(var(--s) / .4);
  padding: 1em;
  margin-block: 1em;
}

.chat-header img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.chat-wrapper {
  padding: 1em .5em;
}
</style>