<template>
  <div
    class="w-screen h-screen container mx-auto p-4 flex flex-col gap-4 overflow-y-auto"
    @submit.prevent="onSubmit()"
  >
    <div class="shadow-md rounded bg-white p-4 relative">
      <button
        type="button"
        class="text-gray-400 rounded-full p-2 flex items-center absolute z-10 top-0 right-0"
        @click="onReset()"
      >
        <span v-if="busy" class="material-icons animate-spin">refresh</span>
        <span v-else class="material-icons">clear</span>
      </button>
      <textarea
        @keyup="onKeyUp($event)"
        class="h-min font-medium text-xl text-gray-800 resize-none w-full focus:outline-none focus:ring focus:border-blue-300"
        placeholder="Type in here..."
        v-model="source"
      ></textarea>
    </div>
    <div
      class="shadow-md rounded p-4 h-min bg-gray-100 text-gray-800 font-bold text-xl relative"
    >
      <span
        v-if="error"
        class="material-icons absolute top-0 right-0 z-10 m-1 text-red-500"
        >error</span
      >
      <div>{{ translation }}</div>
    </div>
    <div class="text-center text-sm text-gray-500" v-if="language">
      {{ language }} => English
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import translateText from "https://aifn.run/fn/228e48d5-7188-4529-bfc6-8430b4ecf8a0.js";
import detectLanguage from "https://aifn.run/fn/d85f993d-26ff-45e0-948a-49811a8a671c.js";

const source = ref("");
const busy = ref(false);
const error = ref(false);
const translation = ref("");
const language = ref("");

async function onSubmit() {
  if (!source.value) {
    translation.value = "";
    return;
  }

  busy.value = true;

  try {
    const text = source.value;
    translation.value = await translateText({ text });
    language.value = await detectLanguage({ text });
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
