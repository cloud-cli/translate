<template>
  <div
    class="w-screen h-screen container mx-auto p-4 flex flex-col gap-4 overflow-y-auto"
    @submit.prevent="onSubmit()"
  >
    <div class="shadow-md rounded bg-white p-2 relative">
      <span
        v-if="error"
        class="material-icons absolute top-0 right-0 z-10 m-1 text-red-500"
        >error</span
      >
      <textarea
        @keyup="onKeyUp($event)"
        class="h-min font-medium text-xl text-gray-800"
        placeholder="Text..."
        v-model="source"
      ></textarea>
    </div>
    <div
      class="shadow-md rounded p-2 h-min bg-gray-100 text-primary font-bold text-xl relative"
    >
      <button
        type="button"
        class="text-primary p-2 flex items-center absolute z-10 top-0 right-0 m-1"
        @click="onReset()"
      >
        <span v-if="busy" class="material-icons animate-spin">refresh</span>
        <span v-else class="material-icons">clear</span>
      </button>
      <div>{{ translation }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import translateText from "https://aifn.run/fn/228e48d5-7188-4529-bfc6-8430b4ecf8a0.js";

const source = ref("");
const busy = ref(false);
const error = ref(false);
const translation = ref("");

async function onSubmit() {
  if (!source.value) return;

  busy.value = true;

  try {
    translation.value = await translateText({ text: source.value });
  } catch {
    error.value = true;
  } finally {
    busy.value = false;
    error.value = false;
  }
}

function debounce(fn, delay = 500) {
  let timeoutId;

  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}

function onKeyUp(event: KeyboardEvent) {
  if (event.code === "Enter" && event.ctrlKey) {
    onSubmit();
  }
}

function onReset() {
  source.value = "";
  translation.value = "";
  error.value = false;
}

watch(source, debounce(onSubmit));
onMounted(() => document.querySelector("textarea")?.focus());
</script>

<style>
.h-min {
  min-height: 45vh
}
</style>
