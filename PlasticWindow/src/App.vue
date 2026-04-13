<template>
  <div class="container mx-auto px-4">
    <div class="text-center mb-8 pt-8">
      <h2 class="text-3xl md:text-4xl lg:text-5xl font-extrabold pb-3">
        Калькулятор окон
      </h2>
      <span class="text-lg text-gray-600">
        Точная цена будет известна после замера на объекте
      </span>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
      <BgCard class="justify-between">
        <img src="/Window.png" alt="" class="w-110 self-center" />
        <span class="font-size-lg text-gray-600">
          Окна <strong>Melke Smart Ultra</strong> при ширине профиля в 65 мм
          сохраняют цену, характерную для более легких 60-миллиметровых систем.
          Превосходный уровень тепло- и шумоизоляции делает данную модель
          идеальным выбором для остекления как городских квартир, так и
          загородных домов.
        </span>
      </BgCard>

      <div class="grid gap-3">
        <BgCard>
          <div
            class="rounded-3xl pb-8 items-top self-center grid grid-cols-1 xl:grid-cols-2 gap-8"
          >
            <div v-for="(config, index) in windowConfigs" :key="index" class="">
              <label class="block text-sm font-bold text-gray-900 mb-2 ml-1">
                {{ config.label }}
              </label>
              <SelectComponent :config="config" />
            </div>
          </div>
          <ExtraOptions :extraOptions="extraOptions" />
        </BgCard>

        <BgCard>
          <div class="flex flex-col gap-2">
            <div
              v-if="windowConfigs.find((opt) => opt.label === 'Тип окна')"
              class="flex justify-between text-gray-500"
            >
              <span>{{
                windowConfigs.find((opt) => opt.label === "Тип окна")
                  .selectedValue
              }}</span>
              <span class="font-bold text-black"
                >{{ getTypePrice("Тип окна") }} ₽/м2</span
              >
            </div>
            <div
              v-if="extraOptions.find((opt) => opt.name === 'Монтаж')?.selected"
              class="flex justify-between text-gray-500"
            >
              <span>Монтаж</span>
              <span class="font-bold text-black"
                >{{
                  extraOptions.find((opt) => opt.name === "Монтаж").price
                }}
                ₽</span
              >
            </div>
            <div
              v-if="
                extraOptions.find(
                  (opt) => opt.name !== 'Монтаж' && opt.selected,
                )?.selected
              "
              class="flex justify-between text-gray-500"
            >
              <span>Аксессуары</span>
              <span class="font-bold text-black"
                >{{
                  extraOptions
                    .filter((opt) => opt.name !== "Монтаж" && opt.selected)
                    .reduce((sum, opt) => sum + opt.price, 0)
                }}
                ₽</span
              >
            </div>

            <div
              class="flex flex-col md:flex-row justify-between md:items-center items-start"
            >
              <span class="text-2xl font-black"
                >Итого: {{ totalPrice }} ₽/м2</span
              >
              <button
                @click="AddToCart"
                class="bg-cyan-400 text-white w-full md:w-auto font-bold px-5 py-3 rounded-2xl hover:brightness-90 transition shadow-2xl cursor-pointer"
              >
                Добавить
              </button>
            </div>
          </div>
        </BgCard>
      </div>
      <BgCard class="col-span-1 md:col-span-2">
        <h2 class="font-bold text-2xl">Список окон</h2>

        <div v-for="value in cart" :key="value.id">
          <div class="grid grid-cols-12 items-center">
            <div class="col-span-2">
              <img class="w-30 m-4 ml-0" :src="value.photo" alt="" />
            </div>
            <div class="flex flex-col text-lg col-span-9 gap-2">
              <h3 class="font-bold">
                Melke
                {{
                  windowConfigs.find(
                    (opt) => opt.label === "Профильная система Melke",
                  ).selectedValue
                }}
              </h3>

              <span>
                {{
                  value.config.map((c) => `${c.label}: ${c.value}`).join(", ")
                }}
                + {{ value.extras.map((e) => e.name).join(", ") }}
              </span>
              <span class="font-bold text-2xl"> {{ value.price }}₽/м2 </span>
            </div>
            <div class="flex flex-col items-end col-span-1 gap-2">
              <button
                class="bg-[#33C5F3] w-12 h-12 p-3 rounded-xl flex justify-center hover:brightness-90 transition cursor-pointer shadow-2xl"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="white"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path
                    d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"
                  ></path>
                  <path
                    d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"
                  ></path>
                </svg>
              </button>

              <button
                @click="DeleteFromCart(value.id)"
                class="bg-[#FF0000] w-12 h-12 p-3 rounded-xl flex justify-center hover:brightness-90 transition cursor-pointer shadow-2xl"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="white"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <polyline points="3 6 5 6 21 6"></polyline>
                  <path
                    d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                  ></path>
                  <line x1="10" y1="11" x2="10" y2="17"></line>
                  <line x1="14" y1="11" x2="14" y2="17"></line>
                </svg>
              </button>
            </div>
          </div>
          <hr />
        </div>
        <div
          class="flex flex-col md:flex-row justify-between items-start gap-4 md:items-center pt-10"
        >
          <div class="flex items-center gap-4">
            <div
              class="bg-cyan-400 rounded-full w-8 h-8 flex items-center justify-center hover:brightness-90 transition cursor-pointer shadow-2xl"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                viewBox="0 0 24 24"
                fill="none"
                stroke="white"
                stroke-width="2.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M12 3v13m0 0l-4-4m4 4l4-4" />
                <path d="M5 21h14" />
              </svg>
            </div>
            <span>Скачать расчёт</span>
          </div>

          <div class="flex flex-col md:flex-row gap-4 items-start md:items-center w-full md:w-auto">
            <div class="font-bold text-2xl mr-8 ">
              Итого: {{ cart.reduce((sum, item) => sum + item.price, 0) }} ₽/м2
            </div>
            <button
              class="bg-cyan-400 px-5 w-full md:w-auto py-3 text-white rounded-2xl font-bold hover:brightness-90 transition cursor-pointer shadow-2xl"
            >
              Вызвать замерщика
            </button>
          </div>
        </div>
      </BgCard>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import BgCard from "./components/BgCard.vue";
import SelectComponent from "./components/SelectComponent.vue";
import ExtraOptions from "./components/ExtraOptions.vue";

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
      { name: "Onix", color: "#0A0A0A", price: 2000 },
    ],
    selectedValue: "нет",
    showSides: true,
    sidePrice: 1000,
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
    sidePrice: 1000,
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

const getTypePrice = (label) => {
  const config = windowConfigs.value.find((c) => c.label === label);
  if (!config) return 0;
  const option = config.options.find((o) => o.name === config.selectedValue);
  return option ? option.price : 0;
};
const extraOptions = ref([
  { name: "Подоконник", price: 1500, selected: false },
  { name: "Декоративная раскладка", price: 2000, selected: false },
  { name: "Отлив", price: 800, selected: false },
  { name: "Монтаж", price: 8000, selected: true },
  { name: "Москитная сетка", price: 1200, selected: false },
  { name: "Детская безопасность (бесплатно)", price: 0, selected: false },
  { name: "Откосы", price: 3000, selected: false },
  { name: "Доставка", price: 1500, selected: true },
]);

const totalPrice = computed(() => {
  const selectPrice = windowConfigs.value.reduce((total, config) => {
    const selectedOption = config.options.find(
      (opt) => opt.name === config.selectedValue,
    );
    let price = selectedOption ? selectedOption.price : 0;
    if (config.showSides && config.selectedValue !== "нет") {
      if (config.sides.inside) price += config.sidePrice;
      if (config.sides.outside) price += config.sidePrice;
    }
    return total + price;
  }, 0);

  const extraPrice = extraOptions.value
    .filter((opt) => opt.selected)
    .reduce((total, opt) => total + opt.price, 0);

  return selectPrice + extraPrice;
});

const AddToCart = () => {
  const newItem = {
    id: Date.now(),
    config: windowConfigs.value.map((config) => ({
      label: config.label,
      value: config.selectedValue,
      sides: config.sides,
    })),
    extras: extraOptions.value.filter((opt) => opt.selected),
    price: totalPrice.value,
    photo: "/Window.png",
  };
  cart.value.push(newItem);
};

const DeleteFromCart = (id) => {
  cart.value = cart.value.filter((item) => item.id !== id);
};

const cart = ref([]);
</script>
