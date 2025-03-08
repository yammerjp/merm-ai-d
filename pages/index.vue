<script setup lang="ts">
import { useChat } from '@ai-sdk/vue';

const { messages, input, handleSubmit } = useChat();
</script>

<template>
  <div class="flex flex-col w-full max-w-md py-24 mx-auto stretch">
    <div v-for="m in messages" :key="m.id" class="whitespace-pre-wrap">
      {{ m.role === 'user' ? 'User: ' : 'AI: ' }}
      {{ m.content }}
    </div>

    <form @submit="handleSubmit">
      <input
        class="fixed bottom-0 w-full max-w-md p-2 mb-8 border border-gray-300 rounded shadow-xl"
        v-model="input"
        placeholder="Say something..."
      />
    </form>
  </div>
</template>

<style scoped>
.fixed {
  position: fixed;
}

.bottom-0 {
  bottom: 0px;
}

.w-full {
  width: 100%;
}

.max-w-md {
  max-width: 28rem; /* 448px */
}

.p-2 {
  padding: 0.5rem; /* 8px */
}

.mb-8 {
  margin-bottom: 2rem; /* 32px */
}

.border {
  border-width: 1px;
}

.border-gray-300 {
  border-color: rgb(209 213 219); /* #D1D5DB */
}

.rounded {
  border-radius: 0.25rem; /* 4px */
}

.shadow-xl {
  --tw-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
  --tw-shadow-colored: 0 25px 50px -12px var(--tw-shadow-color);
  box-shadow: var(--tw-shadow);
}

.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}

.py-24 {
  padding-top: 6rem; /* 96px */
  padding-bottom: 6rem; /* 96px */
}

.mx-auto {
  margin-left: auto;
  margin-right: auto;
}

</style>
