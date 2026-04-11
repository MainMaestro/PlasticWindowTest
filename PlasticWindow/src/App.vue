<template>
  <div class="container mx-auto">
    <div class="text-center mb-8 pt-8">
      <h2 class="text-5xl font-extrabold pb-3">Калькулятор окон</h2>
      <span class="text-lg text-gray-600">
        Точная цена будет известна после замера на объекте
      </span>
    </div>
    <div class="grid grid-cols-2 gap-3">
      <div class="bg-white rounded-3xl p-8 items-center flex flex-col">
        <img src="/Window.png" alt="" />
        <span class="font-size-lg text-gray-600">
          Окна <strong>Melke Smart Ultra</strong> при ширине профиля в 65 мм
          сохраняют цену, характерную для более легких 60-миллиметровых систем.
          Превосходный уровень тепло- и шумоизоляции делает данную модель
          идеальным выбором для остекления как городских квартир, так и
          загородных домов.
        </span>
      </div>

      <div>
        <div class="bg-white rounded-3xl p-8">
          <div class="rounded-3xl pb-8 items-top grid grid-cols-2 gap-5">
            <div v-for="(config, index) in windowConfigs" :key="index" class="">
              <label class="block text-sm font-bold text-gray-900 mb-2 ml-1">
                {{ config.label }}
              </label>

              <Listbox v-model="config.selectedValue">
                <div class="relative">
                  <!-- Кнопка селекта -->
                  <ListboxButton
                    class="relative w-72 flex items-center cursor-pointer rounded-2xl border-2 border-slate-900 bg-white py-4 pl-4 pr-12 text-left text-lg font-medium focus:outline-none focus:ring-2 focus:ring-blue-400"
                  >
                    <span class="block truncate">{{
                      config.selectedValue
                    }}</span
                    ><template
                      v-for="option in config.options"
                      :key="option.name"
                    >
                      <span
                        v-if="
                          option.name === config.selectedValue &&
                          option.description
                        "
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
                      <!-- Иконка стрелочки -->
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
                  <span
                    class="text-xs"
                    v-if="config.label == 'Покрытие Cool Colours'"
                    >Melke Smart Ultra / Melke Evolution</span
                  >
                  <span
                    class="text-xs"
                    v-else-if="config.label == 'Стеклопакет'"
                    >Climatherm (+всем в стандарте без наценки: тихий,
                    теплый)</span
                  >

                  <div
                    v-if="config.showSides && config.selectedValue !== 'нет'"
                    class="flex gap-4 mt-3 ml-1"
                  >
                    <label class="flex items-center gap-2 cursor-pointer group">
                      <input
                        type="checkbox"
                        v-model="config.sides.inside"
                        class="w-4 h-4 rounded border-slate-900 text-slate-900 focus:ring-0 cursor-pointer"
                      />
                      <span class="text-sm text-gray-600 group-hover:text-black"
                        >Внутри</span
                      >
                    </label>

                    <label class="flex items-center gap-2 cursor-pointer group">
                      <input
                        type="checkbox"
                        v-model="config.sides.outside"
                        class="w-4 h-4 rounded border-slate-900 text-slate-900 focus:ring-0 cursor-pointer"
                      />
                      <span class="text-sm text-gray-600 group-hover:text-black"
                        >Снаружи</span
                      >
                    </label>
                  </div>

                  <!-- Выпадающее меню -->
                  <transition
                    leave-active-class="transition duration-100 ease-in"
                    leave-from-class="opacity-100"
                    leave-to-class="opacity-0"
                  >
                    <ListboxOptions
                      class="absolute w-72 mt-2 max-h-60 overflow-auto rounded-2xl bg-white py-2 text-base shadow-xl ring-1 ring-black ring-opacity-5 focus:outline-none z-50"
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
                </div>
              </Listbox>
            </div>
            <!--Доп опции-->
          </div>
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
                  <!-- Иконка галочки (белая), появляется при выборе -->
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
        </div>

        <!--стоимость-->
        <div
          class="p-8 bg-white rounded-3xl mt-4 shadow-sm border border-slate-50"
        >
          <div class="flex flex-col gap-2">
            <div class="flex justify-between text-gray-500">
              <span>:</span>
              <span class="font-bold text-black">{{ totalPrice }} ₽</span>
            </div>
            <div class="flex justify-between text-gray-500">
              <span>Монтаж</span>
              <span class="font-bold text-black">{{ montage }} ₽</span>
            </div>

            <hr class="my-4" />

            <div class="flex justify-between items-end">
              <span class="text-4xl font-black"
                >Итого: {{ totalPrice + montage }} ₽</span
              >
              <button
                class="bg-cyan-400 hover:bg-cyan-500 text-white font-bold py-4 px-10 rounded-2xl transition-all shadow-lg shadow-cyan-100"
              >
                Добавить
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="bg-white p-8">
      <h2>Список окон</h2>
      <div class="flex">
        <div>
          <img class="w-30" src="/Window.png" alt="" />
        </div>
        <div class="flex flex-col">
          <h3>Melke Smart Ultra’65</h3>
          <span
            >Двухстврочатое окно 1 400х1 400 мм / двухстороннее покрытие Cool
            Colours Onix / двухкамерный стеклопакет / подоконник 1 500х300 мм /
            отлив / декоративная раскладка / детская безопасность / откосы /
            доставка/монтаж</span
          >
          {{ totalPrice + montage }}
        </div>
      </div>
      <hr />
    </div>
  </div>
</template>

<script setup>
import {
  Listbox,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
} from "@headlessui/vue";
import { ref, computed } from "vue";

const montage = 8000;
const windowConfigs = ref([
  {
    label: "Тип окна",
    selectedValue: "Двухстворчатое",
    options: [
      { name: "Одностворчатое", price: 5000 },
      { name: "Двухстворчатое", price: 8000 },
      { name: "Трёхстворчатое", price: 12000 },
      { name: "Балконный блок", price: 15000 },
    ],
  },
  {
    label: "Покрытие Cool Colours",
    options: [
      { name: "нет", price: 0 },
      { name: "Onix", color:"#0A0A0A", price: 2000 },
    ],
    selectedValue: "нет",
    showSides: true,
    sides: { inside: false, outside: false },
  },
  {
    label: "Профильная система Melke",
    options: [
      { name: "Lite’60", description: "Лучшая цена", price: 0 },
      { name: "Smart Ultra’65", description: "Новинка", price: 1000 },
      { name: "Lite’70", price: 1000 },
      { name: "Evolution", description: "Хит продаж", price: 2000 },
    ],
    selectedValue: "Lite’60",
  },
  {
    label: "Ламинация",
    options: [
      { name: "нет", price: 0 },
      { name: "Угольный серый", color: "#767676", price: 1000 },
      { name: "Коричневый дуб", color: "#4E2F1B", price: 1500 },
      { name: "Угольно-коричневый", color: "#24110A", price: 1500 },
      { name: "Золотой дуб", color: "#9A7126", price: 2000 },
    ],
    selectedValue: "нет",
    showSides: true,
    sides: { inside: false, outside: false },
  },
  {
    label: "Стеклопакет",
    selectedValue: "Двухкамерный",
    options: [
      { name: "Однокамерный", price: 0 },
      { name: "Двухкамерный", price: 2500 },
    ],
  },
]);

const extraOptions = ref([
  { name: "Подоконник", price: 1500, selected: false },
  { name: "Декоративная раскладка", price: 2000, selected: false },
  { name: "Отлив", price: 800, selected: false },
  { name: "Монтаж", price: 5000, selected: true },
  { name: "Москитная сетка", price: 1200, selected: false },
  { name: "Детская безопасность (бесплатно)", price: 700, selected: false },
  { name: "Откосы", price: 3000, selected: false },
  { name: "Доставка", price: 1500, selected: true },
]);

const totalPrice = computed(() => {
  const selectPrice = windowConfigs.value.reduce((total, config) => {
    const selectedOption = config.options.find(
      (opt) => opt.name === config.selectedValue,
    );
    return total + (selectedOption ? selectedOption.price : 0);
  }, 0);

  const extraPrice = extraOptions.value
    .filter((opt) => opt.selected)
    .reduce((total, opt) => total + opt.price, 0);

  return selectPrice + extraPrice;
});
</script>
