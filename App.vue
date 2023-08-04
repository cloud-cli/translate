<template>
  <div class="w-screen h-screen p-4">
    <form @submit.prevent="onSubmit()" class="container mx-auto">
      <div class="flex flex-col gap-4">
        <textarea
          class="p-2 bg-transparent h-min font-medium text-xl"
          v-model="source"
        ></textarea>
        <div class="p-2 h-min border border-gray-300 text-primary font-bold text-xl overflow-y-auto">
          {{ translation }}
        </div>


        <button
          type="submit"
          :disabled="busy"
          class="bg-primary text-white px-4 py-3 rounded flex items-center mx-auto"
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
import { ref, onMounted } from "vue";
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

onMounted(()=> document.querySelector('textarea')?.focus())
</script>
<style>
.h-min { min-height: 40vh }</style>