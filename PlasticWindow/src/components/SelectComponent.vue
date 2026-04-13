<template>
  <Listbox v-model="config.selectedValue">
    <div class="relative ">
      <ListboxButton
        class="relative w-full  flex items-center cursor-pointer rounded-2xl border-2 border-slate-900 bg-white py-4 pl-4 pr-12 text-left text-lg font-medium focus:outline-none focus:ring-2 focus:ring-blue-400"
      >
        <span class="block truncate">{{ config.selectedValue }}</span
        ><template v-for="option in config.options" :key="option.name">
          <span
            v-if="option.name === config.selectedValue && option.description"
            class="text-sm p-3 rounded-full text-white h-6 ml-2 text-center flex items-center justify-center"
            :class="
              option.description === 'Хит продаж'
                ? 'bg-[#FF00AA]'
                : 'bg-blue-300'
            "
          >
            {{ option.description }}
          </span>
          <span
            v-if="option.color && option.name === config.selectedValue"
            :style="{ backgroundColor: option.color }"
            class="w-5 h-5 rounded-full ml-2"
          ></span>
        </template>

        <span
          class="pointer-events-none absolute inset-y-0 right-0 flex items-center pr-4"
        >
          <svg
            class="h-5 w-5 text-black"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M19 9l-7 7-7-7"
            />
          </svg>
        </span>
      </ListboxButton>
      <transition
        leave-active-class="transition duration-100 ease-in"
        leave-from-class="opacity-100"
        leave-to-class="opacity-0"
      >
        <ListboxOptions
          class="absolute w-full md:w-72 mt-2 max-h-60 overflow-auto rounded-2xl bg-white py-2 text-base shadow-xl ring-1 ring-black ring-opacity-5 focus:outline-none z-50"
        >
          <ListboxOption
            v-for="option in config.options"
            :key="option"
            :value="option.name"
            v-slot="{ active, selected }"
          >
            <li
              class="flex items-center"
              :class="[
                active ? 'bg-blue-100' : 'bg-white',
                'relative cursor-pointer select-none py-2 px-4',
              ]"
            >
              <span
                :class="[
                  selected ? 'font-bold' : 'font-normal',
                  'block truncate',
                ]"
              >
                {{ option.name }} </span
              ><span
                v-if="option.description"
                class="text-sm p-3 rounded-full text-white h-6 ml-2 text-center flex items-center justify-center"
                :class="
                  option.description == 'Хит продаж'
                    ? 'bg-[#FF00AA]'
                    : 'bg-blue-300'
                "
                >{{ option.description }}</span
              >
              <span
                v-if="option.color"
                :style="{ backgroundColor: option.color }"
                class="w-4 h-4 rounded-full ml-2"
              ></span>
            </li>
          </ListboxOption>
        </ListboxOptions>
      </transition>
      <div>
        <span class="text-xs" v-if="config.label == 'Покрытие Cool Colours'"
          >Melke Smart Ultra / Melke Evolution</span
        >
        <span class="text-xs" v-else-if="config.label == 'Стеклопакет'"
          >Climatherm (+всем в стандарте без наценки: тихий, теплый)</span
        >
      </div>
      <div
        v-if="config.showSides && config.selectedValue !== 'нет'"
        class="flex gap-4 mt-3 ml-1"
      >
        <label class="flex items-center gap-2 cursor-pointer group">
          <input
            type="checkbox"
            v-model="config.sides.inside"
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
          <span class="text-sm text-gray-600 group-hover:text-black"
            >Внутри</span
          >
        </label>

        <label class="flex items-center gap-2 cursor-pointer group">
          <input
            type="checkbox"
            v-model="config.sides.outside"
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
          <span class="text-sm text-gray-600 group-hover:text-black"
            >Снаружи</span
          >
        </label>
      </div>
    </div>
  </Listbox>
</template>

<script setup>
import {
  Listbox,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
} from "@headlessui/vue";
const { config } = defineProps({
  config: {
    type: Object,
    required: true,
  },
});
</script>
