<template>
  <div class="contacts py-10 px-4 max-w-2xl mx-auto">
    <p v-if="block.description" class="text-center text-gray-600 italic mb-8">{{ block.description }}</p>

    <div class="grid gap-6 sm:grid-cols-2">
      <div
        v-for="(person, idx) in block.elements"
        :key="idx"
        class="border border-accent/20 rounded-2xl p-6 bg-white shadow-sm text-center flex flex-col items-center gap-3"
      >
        <!-- Avatar placeholder -->
        <div class="w-14 h-14 rounded-full bg-accent/10 flex items-center justify-center text-2xl font-heading text-accent">
          {{ person.name?.charAt(0) }}
        </div>

        <div>
          <p class="font-heading text-xl text-gray-800">{{ person.name }}</p>
          <span class="inline-block text-xs uppercase tracking-widest text-white bg-accent rounded-full px-3 py-0.5 mt-1">
            {{ person.role }}
          </span>
        </div>

        <div class="flex flex-col gap-1.5 text-sm w-full mt-1">
          <a
            v-if="person.phone"
            :href="`tel:${person.phone}`"
            class="flex items-center justify-center gap-2 text-gray-600 hover:text-accent transition-colors"
          >
            <span>📞</span> {{ person.phone }}
          </a>
          <a
            v-if="person.email"
            :href="`mailto:${person.email}`"
            class="flex items-center justify-center gap-2 text-gray-600 hover:text-accent transition-colors"
          >
            <span>✉️</span> {{ person.email }}
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  block: {
    type: Object,
    required: true,
  },
});
</script>
