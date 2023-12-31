<script setup>
import { reactive, watch, onMounted } from 'vue'

const themes = [
  'business',
  'light',
  'dark',
  'cupcake',
  'bumlebee',
  'emerald',
  'corporate',
  'synthwave',
  'retro',
  'cyberpunk',
  'valentine',
  'halloween',
  'garden',
  'forest',
  'aqua',
  'lofi',
  'pastel',
  'fantasy',
  'wireframe',
  'black',
  'luxury',
  'dracula',
  'cmyk',
  'autumn',
  'acid',
  'lemonade',
  'night',
  'coffee',
  'winter',
]

const onChangeTheme = (e) => {
  document.querySelector('html').setAttribute('data-theme', e.target.value)
}

const settings = reactive({
  from: {
    id: 1,
    name: 'Adam',
    profilePicture: '',
  },
  to: {
    id: 2,
    name: 'Eve',
    profilePicture: '',
  },
  messages: [
    {
      id: 1,
      text: "Hi there! I just created a new account here. My name is Adam. I'm excited to meet you.",
      typingTime: 2000,
      waitAfter: 3000,
      type: 'from'
    },
    {
      id: 2,
      text: "Hello Adam! I also created a new account here. My name is Eve. How nice, I'm also thrilled to meet you.",
      typingTime: 2000,
      waitAfter: 3000,
      type: 'to'
    },
    {
      id: 3,
      text: "How are you doing? It can be a bit challenging to be in a new environment, right?",
      typingTime: 2000,
      waitAfter: 3000,
      type: 'from'
    },
    {
      id: 4,
      text: "Yes, you're right. It may seem a bit overwhelming at first, but I think I'll get used to it over time. This internet thing is quite interesting.",
      typingTime: 2000,
      waitAfter: 3000,
      type: 'to'
    }
  ]
})

watch(settings, (val) => {
  localStorage.setItem('settings', JSON.stringify(val))
})

const setProfilePicture = (e, type) => {
  const reader = new FileReader()
  reader.readAsDataURL(e.target.files[0])
  reader.onload = () => {
    settings[type].profilePicture = reader.result
  }
}

const addMessage = () => {
  const lastMessage = settings.messages[settings.messages.length - 1]
  if (lastMessage.type === 'from') {
    settings.messages.push({
      id: lastMessage.id + 1,
      text: '',
      typingTime: 2000,
      waitAfter: 3000,
      type: 'from'
    })
  } else {
    settings.messages.push({
      id: lastMessage.id + 1,
      text: '',
      typingTime: 2000,
      waitAfter: 3000,
      type: 'to'
    })
  }
}

const removeMessage = (index) => {
  settings.messages.splice(index, 1)
}

const onInputMessage = (e, message) => {
  message.typingTime = e.target.value.length * 100
}

const onSave = () => {
  localStorage.setItem('settings', JSON.stringify(settings))
}

onMounted(() => {
  localStorage.setItem('settings', JSON.stringify(settings))
})

</script>

<template>
  <h1 class="text-lg font-bold mb-4 mt-4">Settings</h1>
  <div class="theme mb-6 box">
    Theme
    <select id="theme-changer" class="select select-primary w-full" @change="onChangeTheme">
      <option v-for="theme in themes" :value="theme">{{ theme }}</option>
    </select>
  </div>
  <div>
    <h2 class="font-bold">Screen Information</h2>
    <form class="flex flex-col gap-2">
      <div class="from box flex flex-col">
        From
        <input class="input input-accent" placeholder="Name" type="text" v-model="settings.from.name">
        <img v-if="settings.from.profilePicture" :src="settings.from.profilePicture" class="p-2 w-32 h-32">
        <input class="input input-accent" type="file" @change="setProfilePicture($event, 'from')">
      </div>
      <div class="to box flex flex-col">
        To
        <input class="input input-accent" placeholder="Name" type="text" v-model="settings.to.name">
        <img v-if="settings.to.profilePicture" :src="settings.to.profilePicture" class="p-2 w-32 h-32">
        <input class="input input-accent" type="file" @change="setProfilePicture($event, 'to')">
      </div>
    </form>
  </div>

  <div class="messages mt-5">
    <h2 class="font-bold">Messages</h2>
    <div class="message">
      <div class="from">
        <div class="messages">
          <div class="box" v-for="(message, index) in settings.messages" :key="message.id">
            <textarea class="textarea w-full" v-model="message.text" @input="onInputMessage($event, message)"></textarea>
            <label class="label typing-time">
              Typing side
              <select class="select select-primary" v-model="message.type">
                <option value="from">From</option>
                <option value="to">To</option>
              </select>
            </label>
            <label class="label typing-time">
              <span>
                Typing time <i>(ms)</i>
              </span>
              <input type="number" v-model="message.typingTime">
            </label>
            <label class="label wait-after">
              <span>
                Wait after <i>(ms)</i>
              </span>
              <input type="number" v-model="message.waitAfter">
            </label>
            <button v-if="settings.messages.length > 1" class="btn  btn-sm" @click="removeMessage(index)">
              x Remove Message
            </button>
          </div>
          <button class="btn mt-2" @click="addMessage()">
            + Add Message
          </button>
        </div>
      </div>
    </div>
  </div>

  <div class="btn-save">
    <div class="flex justify-between items-end gap-5">
      <div class="flex justify-between w-full text-xs">
        <span>
          Developed by <a href="https://safagayret.com"><strong>Safa Gayret</strong></a>
        </span>
        <span>
          Code <a href="https://github.com/gayret/fake-chat-for-movie"><strong>on GitHub</strong></a>
        </span>
      </div>
      <button class="btn btn-primary mt-10" @click="onSave">
        Save
      </button>
    </div>
  </div>
</template>

<style scoped>
label {
  display: flex;
  gap: 1em;
  align-items: center;
  justify-content: space-between;
}

.btn-save {
  position: sticky;
  bottom: 0;
  background: linear-gradient(180deg, transparent 0%, hsl(var(--b1)) 100%);
  padding: 1em 0;
}

textarea {
  border: 1px solid hsl(var(--bc));
}

.messages {
  display: grid;
  gap: 1em;
}
</style>