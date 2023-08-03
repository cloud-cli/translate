<template>
  <form
    @submit.prevent="onSubmit()"
    class="flex flex-col container mx-auto p-4"
  >
    <textarea
      class="p-2 bg-transparent font-bold text-xl my-4 h-20"
      v-model="source"
    ></textarea>
    <div class="p-2 bg-gray-200 my-4 text-xl"></div>
    <div class="text-center">
      <button
        type="submit"
        :disabled="busy"
        class="bg-blue-500 text-white px-4 py-3"
      >
        <span v-if="busy" class="material-icons animate-spin">refresh</span>
        <span v-else  class="material-icons">translate</span>
      </button>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
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
</script>
