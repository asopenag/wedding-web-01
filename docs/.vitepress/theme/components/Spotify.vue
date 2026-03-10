<template>
  <div class="spotify py-10 px-4 max-w-2xl mx-auto text-center">
    <p v-if="block.description" class="text-gray-600 mb-6 italic">{{ block.description }}</p>
    <div class="rounded-2xl overflow-hidden shadow-md border border-accent/10">
      <iframe
        v-if="embedUrl"
        :src="embedUrl"
        width="100%"
        height="380"
        frameborder="0"
        allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
        loading="lazy"
        class="block"
      ></iframe>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  block: {
    type: Object,
    required: true,
  },
});

const embedUrl = computed(() => {
  if (!props.block.url) return null;
  // Convert https://open.spotify.com/playlist/ID to https://open.spotify.com/embed/playlist/ID
  return props.block.url.replace("open.spotify.com/", "open.spotify.com/embed/");
});
</script>
