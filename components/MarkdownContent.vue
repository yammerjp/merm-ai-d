<script setup lang="ts">
import type { Node } from 'mdast';

defineProps<{
    node: Node[];
}>();
</script>

<template>
  <template v-if="typeof node.value === 'string'"><span>{{ node.value }}</span></template>
  <template v-else-if="Array.isArray(node.children)">
    <template v-for="(c, idx) in node.children" :key="idx" v-memo="[JSON.stringify(c)]">  
      <MarkdownNode :node="c" />
    </template>
  </template>
  <template v-else>
    {{ JSON.stringify(node) }}
  </template>
</template>
