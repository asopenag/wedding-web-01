<template>
  <div class="rsvp py-12 px-4 max-w-xl mx-auto">
    <div v-if="submitted" class="text-center py-16">
      <div class="text-6xl mb-4">💌</div>
      <h3 class="text-2xl font-heading text-accent mb-3">¡Gracias por confirmar!</h3>
      <p class="text-gray-600">Hemos recibido tu confirmación. ¡Nos vemos el 13 de septiembre!</p>
      <button @click="submitted = false" class="mt-6 text-sm text-accent underline underline-offset-4 hover:opacity-75 transition-opacity">
        Enviar otra respuesta
      </button>
    </div>

    <form v-else @submit.prevent="handleSubmit" class="space-y-5">
      <p v-if="block.deadline" class="text-sm text-center text-gray-500 italic">
        Por favor confirma antes del <strong>{{ block.deadline }}</strong>
      </p>

      <!-- Asistencia -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">¿Asistirás? *</label>
        <select v-model="form.attending" required class="w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-accent/40 bg-white">
          <option value="">Selecciona...</option>
          <option value="Sí, asistiré">Sí, asistiré</option>
          <option value="No podré asistir">No podré asistir</option>
        </select>
      </div>

      <!-- Nombre -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Nombre completo *</label>
        <input
          v-model="form.name"
          type="text"
          required
          placeholder="Tu nombre"
          class="w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-accent/40"
        />
      </div>

      <!-- Número de invitados -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Número de asistentes</label>
        <input
          v-model.number="form.guests"
          type="number"
          min="1"
          max="10"
          class="w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-accent/40"
        />
      </div>

      <!-- Restricciones dietéticas -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Restricciones dietéticas o alergias</label>
        <textarea
          v-model="form.dietary"
          rows="3"
          placeholder="Indica si tienes alguna alergia o restricción alimentaria..."
          class="w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-accent/40 resize-none"
        ></textarea>
      </div>

      <button
        type="submit"
        class="w-full bg-accent text-white font-medium py-3 rounded-lg hover:opacity-90 transition-opacity tracking-wide"
      >
        Confirmar asistencia
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";

const props = defineProps({
  block: {
    type: Object,
    required: true,
  },
});

const submitted = ref(false);

const form = reactive({
  attending: "",
  name: "",
  guests: 1,
  dietary: "",
});

function handleSubmit() {
  const email = props.block.email || "";
  const subject = encodeURIComponent("Confirmación de asistencia — Boda María & Carlos");
  const body = encodeURIComponent(
    `Asistencia: ${form.attending}\nNombre: ${form.name}\nNúmero de asistentes: ${form.guests}\nRestricciones dietéticas: ${form.dietary || "Ninguna"}`
  );
  window.open(`mailto:${email}?subject=${subject}&body=${body}`, "_blank");
  submitted.value = true;
}
</script>
