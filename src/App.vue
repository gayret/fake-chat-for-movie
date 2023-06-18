<script setup>
import Setting from './views/Setting.vue'
import Chat from './views/Chat.vue'
import { reactive } from 'vue'

const state = reactive({
  page: 'setting',
})

const togglePage = () => {
  state.page = state.page === 'setting' ? 'chat' : 'setting'
}

</script>

<template>
  <div class="wrapper">
    <div class="layout">
      <header>
        <button class="btn" @click="togglePage">
          {{ state.page === 'setting' ? 'Show Chat' : 'Settings' }}
        </button>
      </header>
      <main>
        <section v-show="state.page === 'setting'">
          <Setting />
        </section>
        <section v-if="state.page === 'chat'">
          <Chat />
        </section>
      </main>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
}

.layout {
  width: 90vw;
  margin-inline: auto;
}
</style>