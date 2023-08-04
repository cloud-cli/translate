<template>
  <div class="w-screen h-screen p-4">
    <form @submit.prevent="onSubmit()" class="container mx-auto">
      <div class="flex flex-col md:flex-row">
        <textarea
          class="p-2 bg-transparent font-medium text-xl my-4 h-20"
          v-model="source"
        ></textarea>
        <hr class="my-4 mx-4">
        <div class="p-2 border border-gray-300 text-primary font-bold my-4 text-xl" v-if="translation">
          {{ translation }}
        </div>
      </div>

      <div class="text-center mt-4">
        <button
          type="submit"
          :disabled="busy"
          class="bg-primary text-white px-4 py-3 rounded"
        >
          <span v-if="busy" class="material-icons animate-spin">refresh</span>
          <span v-else class="material-icons">translate</span>
          <span>Translate</span>
        </button>
      </div>
    </form>
  </div>
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
