<script setup lang="ts">
import type { Node } from 'mdast';

defineProps<{
    node: Node[];
}>();
</script>

<template>
  <template v-if="node.value"><span>{{ node.value }}</span></template>
  <template v-else-if="Array.isArray(node.children)">
    <template v-for="c in node.children" :key="JSON.stringify(c.start)-JSON.stringify(c.end)">  
      <MarkdownNode :node="c" />
    </template>
  </template>
  <template v-else>
    not supported: {{ JSON.stringify(node) }}
  </template>
</template>
