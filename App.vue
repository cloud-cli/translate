<template>
  <form @submit.prevent="onSubmit()" class="flex flex-col">
    <textarea v-bind="source" class="p-2 bg-transparent my-4 h-20"></textarea>
    <div class="p-2 bg-gray-200 my-4"></div>
    <div class="text-center">
      <button
        type="submit"
        :disabled="busy"
        class="bg-blue-400 text-white px-4 py-3"
      >
        <span v-if="busy" class="material-icons">refresh</span>
        <span v-else>Go!</span>
      </button>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
import translateText from "https://aifn.run/fn/228e48d5-7188-4529-bfc6-8430b4ecf8a0.js";
const source = ref("");
const translation = ref("");

async function onSubmit() {
  translation.value = await translateText({ text: source.value });
}
</script>
