<script setup lang="ts">
import { ref } from "vue";

const props = defineProps<{
  modelValue: string;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: string): void;
}>();
</script>

<template>
  <div class="h-1/2 lg:h-full w-full flex flex-col">
    <header class="bg-blue-700 p-4">
      <h1 class="text-1xl font-bold text-white">Markdown Editor</h1>
    </header>

    <main class="px-4 mt-2">
      <textarea
        class="w-full h-full outline-none overflow-y-auto resize-none"
        placeholder="Type your markdown here..."
        :value="props.modelValue"
        @input="
          emit(
            'update:modelValue',
            ($event.target as HTMLTextAreaElement).value
          )
        "
      ></textarea>
    </main>
  </div>
</template>
