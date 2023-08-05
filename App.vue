<template>
  <form
    class="w-screen h-screen container mx-auto p-4 flex flex-col gap-4"
    @submit.prevent="onSubmit()"
  >
    <textarea
      @keyup="onKeyUp($event)"
      class="p-2 bg-white h-min font-medium text-xl rounded-md text-gray-800"
      placeholder="Text..."
      v-model="source"
    ></textarea>
    <button
      type="submit"
      :disabled="busy"
      class="bg-primary text-white px-4 py-3 rounded flex items-center mx-auto"
    >
      <span v-if="busy" class="material-icons animate-spin">refresh</span>
      <span v-else class="material-icons">translate</span>
      <span>Translate</span>
    </button>
    <div
      class="p-2 h-min bg-gray-100 rounded-md text-primary font-bold text-xl overflow-y-auto"
    >
      {{ translation }}
    </div>
  </form>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import translateText from "https://aifn.run/fn/228e48d5-7188-4529-bfc6-8430b4ecf8a0.js";

const source = ref("");
const busy = ref(false);
const translation = ref("");

async function onSubmit() {
  if (!source.value) return;

  busy.value = true;

  try {
    translation.value = await translateText({ text: source.value });
  } finally {
    busy.value = false;
  }
}

function debounce(fn, delay = 500) {
  let timeoutId;

  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}


watch(source, debounce(onSubmit))


function onKeyUp(event: KeyboardEvent) {
  if (event.code === "Enter" && event.ctrlKey) {
    onSubmit();
  }
}
onMounted(() => document.querySelector("textarea")?.focus());
</script>

<style>
.h-min {
  min-height: 35vh
}
</style>
