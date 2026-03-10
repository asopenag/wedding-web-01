<template>
  <div class="countdown py-12 px-4 text-center">
    <h2 v-if="block.title" class="text-3xl mb-8 font-heading text-accent">{{ block.title }}</h2>
    <div class="flex justify-center gap-6 flex-wrap">
      <div
        v-for="unit in units"
        :key="unit.label"
        class="countdown-tile flex flex-col items-center bg-white/80 backdrop-blur-sm border border-accent/20 rounded-2xl px-6 py-4 min-w-[80px] shadow-sm"
      >
        <span class="countdown-number text-5xl font-heading text-accent leading-none">{{ unit.value }}</span>
        <span class="countdown-label text-xs uppercase tracking-widest text-gray-500 mt-2">{{ unit.label }}</span>
      </div>
    </div>
    <p v-if="expired" class="mt-8 text-xl text-accent font-heading italic">¡El gran día ha llegado! 🎉</p>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const props = defineProps({
  block: {
    type: Object,
    required: true,
  },
});

const now = ref(Date.now());
let timer = null;

onMounted(() => {
  timer = setInterval(() => {
    now.value = Date.now();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(timer);
});

const target = computed(() => {
  if (!props.block.date) return null;
  return new Date(props.block.date + "T00:00:00").getTime();
});

const diff = computed(() => {
  if (!target.value) return 0;
  return Math.max(0, target.value - now.value);
});

const expired = computed(() => diff.value === 0);

const units = computed(() => {
  const total = Math.floor(diff.value / 1000);
  const days = Math.floor(total / 86400);
  const hours = Math.floor((total % 86400) / 3600);
  const minutes = Math.floor((total % 3600) / 60);
  const seconds = total % 60;

  return [
    { value: String(days).padStart(2, "0"), label: "días" },
    { value: String(hours).padStart(2, "0"), label: "horas" },
    { value: String(minutes).padStart(2, "0"), label: "min" },
    { value: String(seconds).padStart(2, "0"), label: "seg" },
  ];
});
</script>
