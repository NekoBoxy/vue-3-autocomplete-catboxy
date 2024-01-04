<template>
  <div class="bg-gray-50 min-w-screen min-h-screen flex justify-center items-center">
    <div class="max-w-xs relative space-y-3">
      <!-- <label for="search" class="text-gray-900">
        Type the name of a country to search
      </label> -->

      <input type="text" id="search" v-model="searchTerm" placeholder="Type here..."
        class="p-3 mb-0.5 w-full border border-gray-300 rounded">

      <ul v-if="searchCountries.length"
        class="w-full rounded bg-white border border-gray-300 px-4 py-2 space-y-1 absolute z-10">
        <!-- 提示比對到多少個選項 -->
        <li class="px-1 pt-1 pb-2 font-bold border-b border-gray-200">
          Showing {{ searchCountries.length }} results
        </li>
        <!-- 選項們 -->
        <li v-for="country in searchCountries" :key="country.name" @click="selectCountry(country.name)"
          class="cursor-pointer hover:bg-gray-100 p-1">
          {{ country.name }}
        </li>
      </ul>
      <!-- 標示使用者選了哪個選項 -->
      <!-- <p v-if="selectedCountry" class="text-lg pt-2 absolute">
        You have selected: <span class="font-semibold">{{ selectedCountry }}</span>
      </p> -->
    </div>
  </div>
</template>

<script setup>
import countries from './data/countries.json'
import { ref, computed } from 'vue'

let searchTerm = ref(''); // 與 input 雙向綁定
let selectedCountry = ref('') // 被選擇的選項

const searchCountries = computed(() => {
  if (searchTerm.value === '') {
    return []
  }

  // 配對數
  let matches = 0

  const result = countries.filter((country) => {
    // 若 country (轉小寫後) 包含了 searchTerm (轉小寫後) 的值，且配對數高於十
    if (country.name.toLowerCase().includes(searchTerm.value.toLowerCase()) && matches < 10) {
      matches++;
      return country; // 並回傳此 country 名稱
    }
  });

  if (result.length === 1 && result[0].name === searchTerm.value) {
    // 當配對結果只有一筆，且其值等於 input 的輸入值
    // 回傳空陣列代表"不顯示搜尋結果"
    return [];
  }

  return result;
});

// 處理被選擇的搜尋結果選項
const selectCountry = (country) => {
  selectedCountry.value = country;
  searchTerm.value = country;
}
</script>
