<script setup>
import { ref } from "vue";

const text = ref("");
const searchQuery = ref("");
const searchResult = ref(null);
const replaceFrom = ref("");
const replaceTo = ref("");
const replacedText = ref("");
const sortedWords = ref([]);

const searchWord = () => {
  if (!searchQuery.value) return;
  const regex = new RegExp(`\\b${searchQuery.value}\\b`, "gi");
  const matches = text.value.match(regex);
  searchResult.value = matches ? matches.length : 0;
};

const replaceWord = () => {
  if (!replaceFrom.value || !replaceTo.value) return;
  const regex = new RegExp(`\\b${replaceFrom.value}\\b`, "gi");
  replacedText.value = text.value.replace(regex, replaceTo.value);
};

const sortWords = () => {
  const words = text.value.toLowerCase().match(/\w+/g);
  sortedWords.value = words ? [...new Set(words)].sort() : [];
};
</script>

<template>
  <div class="container px-5 py-10 mx-auto">
    <div class="lg:w-1/2 md:w-2/3 mx-auto">
      <textarea
        v-model="text"
        placeholder="Masukkan teks di sini..."
        class="w-full rounded-xl border border-gray-300 h-40 p-3 leading-6 outline-none"
      ></textarea>

      <div class="my-4">
        <h1 class="text-xl font-bold mb-2">Cari Kata</h1>
        <div class="flex items-center gap-1 sm:flex-row flex-col">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Masukkan kata yang ingin dicari..."
            class="w-full rounded-xl border border-gray-300 py-1 px-3 leading-8 outline-none"
          />
          <button
            @click="searchWord"
            class="w-full lg:w-1/3 text-white bg-blue-500 border-0 py-2 px-6 focus:outline-none hover:bg-blue-600 rounded-xl cursor-pointer"
          >
            Cari
          </button>
        </div>
        <p v-if="searchResult !== null" class="text-gray-500">
          Jumlah kata
          <span class="font-semibold text-red-500">{{ searchQuery }}</span> :
          {{ searchResult }}
        </p>
      </div>

      <div class="my-4">
        <h1 class="text-xl font-bold mb-2">Ganti Kata</h1>
        <div class="flex items-center gap-1 sm:flex-row flex-col">
          <input
            v-model="replaceFrom"
            type="text"
            placeholder="Kata yang diganti"
            class="w-full rounded-xl border border-gray-300 py-1 px-3 leading-8 outline-none"
          />
          <input
            v-model="replaceTo"
            type="text"
            placeholder="Ganti dengan"
            class="w-full rounded-xl border border-gray-300 py-1 px-3 leading-8 outline-none"
          />
          <button
            @click="replaceWord"
            class="w-full lg:w-1/3 text-white bg-blue-500 border-0 py-2 px-6 focus:outline-none hover:bg-blue-600 rounded-xl cursor-pointer"
          >
            Ganti
          </button>
        </div>
        <div
          class="border-gray-500 border p-2 rounded-md mt-4"
          v-if="replacedText"
        >
          <h2 class="mb-2 text-gray-500">
            Hasil setelah kata
            <span class="font-semibold text-red-500">{{ replaceFrom }}</span>
            diganti menjadi
            <span class="font-semibold text-red-500">{{ replaceTo }}</span> :
          </h2>
          <p class="leading-relaxed text-justify">
            {{ replacedText }}
          </p>
        </div>
      </div>

      <div class="my-4">
        <h1 class="text-xl font-bold mb-2">Urutkan Kata</h1>
        <div class="flex items-center gap-1 sm:flex-row flex-col">
          <button
            @click="sortWords"
            class="w-full lg:w-1/3 text-white bg-blue-500 border-0 py-2 px-6 focus:outline-none hover:bg-blue-600 rounded-xl cursor-pointer"
          >
            Urutkan
          </button>
        </div>
        <div
          class="border-gray-500 border p-2 rounded-md mt-4"
          v-if="sortedWords.length"
        >
          <p class="leading-relaxed text-justify">
            {{ sortedWords.join(", ") }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
