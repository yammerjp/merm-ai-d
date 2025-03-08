<script setup lang="ts">
import { useChat } from '@ai-sdk/vue';

const { messages, input, handleSubmit } = useChat();
</script>

<template>
    <div class="chat-page">
        <div class="chat-container">
            <div v-for="m in messages" :key="m.id" :class="['chat-message', m.role === 'user' ? 'user-message' : 'ai-message']">
            {{ m.content }}
            </div>
        </div>

        <div class="input-container">
            <form @submit="handleSubmit">
                <input
                    v-model="input"
                    placeholder="Say something..."
                />
            </form>
        </div>
    </div>
</template>

<style scoped>
.chat-page {
  height: 100%;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  max-width: 768px;
  margin: 0 auto;
}

.chat-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  padding: 10px;
  padding-bottom:80px;
}

.chat-message {
  padding: 1em;
  margin: .5em;
}

.user-message {
  align-self: flex-end;
  background-color: #f3f3f3;
  border-radius: 1em;
}

.ai-message {
  align-self: flex-start;
}

.input-container {
  display: flex;
  margin: 0 auto;
  padding: 1em;
  position: fixed;
  bottom: 0;
  width: 100%;
  max-width: 768px;
  background: white;
  box-sizing: border-box;
  box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
  border: 1px solid #ccc;
  border-radius: 1em; 
}

form {
    width: 100%;
}

input {
  width: 100%;
  outline: none;
  border: none;
  box-sizing: border-box;
}
</style>
