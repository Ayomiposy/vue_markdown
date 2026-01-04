<script setup lang="ts">
import HelloWorld from "./components/HelloWorld.vue";
import Editor from "./components/Editor.vue";
import Preview from "./components/Preview.vue";
import { marked } from "marked";
import { ref } from "vue";

const markedText = ref("");
</script>

<template>
  <div class="flex">
    <Editor v-model="markedText" class="w-1/2" />
    <Preview
      :markedText="markedText"
      class="w-1/2 border-l-2 border-gray-300"
    />
  </div>
</template>
