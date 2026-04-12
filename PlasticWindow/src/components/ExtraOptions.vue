<template>
  <div>
    <h3>Дополнительные опции</h3>
    <div class="grid grid-cols-2 gap-4 mt-3">
      <label
        v-for="extra in extraOptions"
        :key="extra.name"
        class="flex items-center gap-3 cursor-pointer group"
      >
        <div class="relative flex items-center">
          <input
            type="checkbox"
            v-model="extra.selected"
            class="peer h-4 w-4 cursor-pointer appearance-none rounded border transition-all"
          />
          <svg
            class="absolute h-2.5 w-2.5 bg-white text-black opacity-0 peer-checked:opacity-100 ml-0.5 pointer-events-none"
            xmlns="http://w3.org"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="4"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <polyline points="20 6 9 17 4 12"></polyline>
          </svg>
        </div>
        <span
          class="text-sm font-medium text-gray-700 group-hover:text-black transition-colors"
        >
          {{ extra.name }}
        </span>
      </label>
    </div>
  </div>
</template>

<script setup>
defineProps({
  extraOptions: {
    type: Object,
    required: true,
  },
});
</script>
