<template>
  <router-link :to="'/availabilities/' + id">
    <div class="bg-gray-200 p-6 rounded-lg">
      <h3
        class="tracking-widest text-red-500 text-xs font-medium title-font"
      >
        {{ `${firstName} ${lastName}` }}
      </h3>
      <h2 class="text-lg text-gray-900 font-medium title-font mb-4">
        {{ jobRole }}
      </h2>
      <p class="leading-relaxed text-base mb-2">
        {{ description }}
      </p>
      <p class="leading-relaxed text-base">
        {{ new Date(timeFrom).toLocaleString() }} - {{ new Date(timeUntil).toLocaleString() }}
      </p>
    </div>
  </router-link>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    id: {
      type: String,
      required: true,
    },
    firstName: {
      type: String,
      required: true,
    },
    lastName: {
      type: String,
      required: true,
    },
    jobRole: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    timeFrom: {
      type: Date,
      required: true,
    },
    timeUntil: {
      type: Date,
      required: true,
    },
    booked: {
      type: Boolean,
      required: true,
    },
  },
});
</script>
