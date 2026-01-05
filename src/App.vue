<script setup lang="ts">
import HelloWorld from "./components/HelloWorld.vue";
import Editor from "./components/Editor.vue";
import Preview from "./components/Preview.vue";
import { marked } from "marked";
import { ref } from "vue";

const markedText = ref("");
</script>

<template>
  <div class="flex flex-col lg:flex-row h-screen">
    <Editor v-model="markedText" class="w-full sm:w-full" />
    <Preview
      :markedText="markedText"
      class="w-full sm:w-full border-l-2 border-gray-300"
    />
  </div>
</template>
