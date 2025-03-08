<script setup lang="ts">
import { useChat } from '@ai-sdk/vue';
import type { Root } from 'mdast';
import { unified } from 'unified';
import remarkParse from 'remark-parse';
import remarkGfm from 'remark-gfm';
import { computedAsync } from '@vueuse/core';
import MarkdownNode from '@/components/MarkdownNode.vue';
const { messages, input, handleSubmit } = useChat({
  initialMessages: [
    {
      id: 'default-ai',
      role: 'assistant',
      content: `
# タイトル

## セクション1

これは段落です。Markdownでは、**太字**や*斜体*を使うことができます。

- リストアイテム1
- リストアイテム2
  - サブアイテム1
  - サブアイテム2

1. 番号付きリストアイテム1
2. 番号付きリストアイテム2

### セクション2

> これは引用です。

[リンクのテキスト](https://example.com)

![代替テキスト](https://via.placeholder.com/150)

\`\`\`python
# これはコードブロックです
def hello_world():
    print("Hello, world!")
\`\`\`

- [ ] 未完了のタスク
- [x] 完了したタスク

---

## セクション3

**テーブルの例**

| ヘッダー1 | ヘッダー2 |
|-----------|-----------|
| 行1      | 行1のデータ |
| 行2      | 行2のデータ |
    `,
    }
  ]
});

const markdownParser = unified().use(remarkParse).use(remarkGfm);

type StructuredMessage = {
  id: string;
  isAssistant: boolean;
  content: string;
  mdast: Root;
}

const structuredMessages = computedAsync(async (): Promise<StructuredMessage[]> => {
  return await Promise.all(messages.value.map(async (m) => {
    return {
      id: m.id,
      isAssistant: m.role === 'assistant',
      content: m.content,
      mdast: await markdownParser.parse(m.content)
    }
  }))
})
</script>

<template>
    <div class="chat-page">
        <div class="chat-container">
            <template v-for="m in structuredMessages" :key="m.id">
                <div v-if="m.isAssistant" class="chat-message ai-message">
                    <MarkdownNode :node="m.mdast"/>
                </div>
                <div v-else class="chat-message user-message">
                    {{ m.content }}
                </div>
            </template>
        </div>

        <div class="input-container">
            <form @submit="handleSubmit">
                <div class="input-wrapper">
                    <input
                        v-model="input"
                        placeholder="質問してみましょう"
                    />
                    <button type="submit" aria-label="送信">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                            <line x1="12" y1="19" x2="12" y2="5"></line>
                            <polyline points="5 12 12 5 19 12"></polyline>
                        </svg>
                    </button>
                </div>
            </form>
        </div>
    </div>
</template>

<style scoped>
.chat-page {
  height: 100%;
  margin: 0;
  padding: 0 1em;
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
  padding-bottom: 120px;
}

.chat-message {
  margin: 1em 0;
}

.user-message {
  padding: 1em;
  align-self: flex-end;
  background-color: #f3f3f3;
  border-radius: 2em;
  white-space: pre-wrap;
}

.ai-message {
  align-self: flex-start;
}

.input-container {
  display: flex;
  margin-bottom: 1em;
  padding: 1em;
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 2em);
  max-width: 768px;
  background: white;
  box-sizing: border-box;
  box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
  border: 1px solid #ccc;
  border-radius: 2em; 
}

form {
    width: 100%;
}

.input-wrapper {
    display: flex;
    gap: 8px;
    width: 100%;
}

button {
    width: 40px;
    height: 40px;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #01172b;
    color: white;
    border: none;
    border-radius: 50%;
    cursor: pointer;
}

button:hover {
    background-color: #44505b;
}

button svg {
    width: 20px;
    height: 20px;
}

input {
    flex: 1;
    width: auto;
    outline: none;
    border: none;
    box-sizing: border-box;
}
</style>
