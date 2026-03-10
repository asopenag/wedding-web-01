<template>
  <div class="registry py-10 px-4 max-w-2xl mx-auto">
    <p v-if="block.description" class="text-center text-gray-600 italic mb-8">{{ block.description }}</p>

    <div class="space-y-4">
      <div
        v-for="(item, idx) in block.items"
        :key="idx"
        class="border border-accent/20 rounded-2xl p-6 bg-white shadow-sm"
      >
        <div class="flex items-start justify-between gap-4 flex-wrap">
          <div>
            <p class="text-xs uppercase tracking-widest text-gray-400 mb-1">{{ item.bank }}</p>
            <p class="font-heading text-xl text-gray-800">{{ item.name }}</p>
            <p class="mt-2 font-mono text-sm text-gray-700 tracking-wider">{{ formatIban(item.iban) }}</p>
          </div>
          <button
            @click="copyIban(item.iban, idx)"
            class="flex items-center gap-2 text-sm text-accent border border-accent/30 rounded-lg px-4 py-2 hover:bg-accent/5 transition-colors shrink-0"
          >
            <span>{{ copied === idx ? "✓ Copiado" : "Copiar IBAN" }}</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  block: {
    type: Object,
    required: true,
  },
});

const copied = ref(null);

function formatIban(iban) {
  // Format IBAN in groups of 4 for readability
  return (iban || "").replace(/\s/g, "").replace(/(.{4})/g, "$1 ").trim();
}

async function copyIban(iban, idx) {
  try {
    await navigator.clipboard.writeText(iban.replace(/\s/g, ""));
    copied.value = idx;
    setTimeout(() => {
      if (copied.value === idx) copied.value = null;
    }, 2500);
  } catch (e) {
    // Fallback: select text
    console.error("Clipboard error:", e);
  }
}
</script>
