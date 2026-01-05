<script setup lang="ts">
import { marked } from "marked";
import DOMPurify from "dompurify";
import { computed } from "vue";
import { onMounted, onUnmounted, ref } from "vue";

const props = defineProps<{
  markedText: string;
}>();

const markdown = ref("");

const renderedMarkdown = computed(() => {
  const rawHtml = marked.parse(props.markedText);
  return DOMPurify.sanitize(rawHtml as string);
});

// Optional: Display current time in the preview header
const time = ref(new Date().toLocaleTimeString());

let interval: ReturnType<typeof setInterval>;

onMounted(() => {
  interval = setInterval(() => {
    time.value = new Date().toLocaleTimeString();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<template>
  <div class="w-full flex flex-col sm:h-1/2 lg:h-full">
    <header class="bg-blue-700 p-4 flex justify-between items-center">
      <h1 class="text-1xl font-bold text-white">Markdown Preview</h1>
      <div class="bg-white p-[2px] px-[8px] text-blue-700 rounded-sm text-sm">
        {{ time }}
      </div>
    </header>

    <main class="px-4 w-full mt-2">
      <div
        class="w-full h-full prose max-w-none overflow-y-auto"
        placeholder="Type your markdown here..."
        v-html="renderedMarkdown"
      ></div>
    </main>
  </div>
</template>
