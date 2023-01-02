<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from "vue";
const show = ref<boolean>(true);
const data = ref<[{ name: string }]>([
  {
    name: "Afman42",
  },
  {
    name: "Armandos42",
  },
  {
    name: "Code_500_page",
  },
  {
    name: "mando_42_man",
  },
]);
const searchText = ref<string>("");
function openShow(showBool: boolean) {
  show.value = showBool;
}
const filteredData = computed(() => {
  if (searchText.value !== "") {
    return data.value.filter((item: { name: string }) => {
      return item.name.toLowerCase().includes(searchText.value);
    });
  } else {
    return data.value;
  }
});
const keyboardTap = (e: Event) => {
  if (event.ctrlKey && event.code === "Comma") {
    show.value = !show.value;
  }

  if (event.code === "Escape") {
    show.value = true;
  }
};
onMounted(() => {
  window.addEventListener("keyup", keyboardTap);
});
onUnmounted(() => {
  window.removeEventListener("keyup", keyboardTap);
});
</script>

<template>
  <div class="flex min-h-screen justify-center items-center">
    <div class="flex flex-col">
      <h3 class="text-center mb-2">
        FullScreen Search Popup, <br />
        di klik atau tekan keyboard
        <span class="bg-green-200 rounded-full px-2">CTRL + ,</span>
      </h3>
      <div
        class="flex flex-row cursor-pointer border-1 px-2 py-2 border-black bg-gray-200 rounded-full"
        @click="openShow(false)"
      >
        <div class="w-96 bg-gray-200 pl-3">Search</div>
        <h3 class="bg-green-200 rounded-full px-2">CTRL + ,</h3>
      </div>
    </div>
    <transition name="no-item-opacity">
      <div
        class="fixed top-0 left-0 w-screen h-screen bg-gray-200 z-90"
        v-if="!show"
      >
        <span
          class="cursor-pointer text-6xl text-white hover:text-amber-500 absolute right-5 top-5"
          title="Close"
          @click="openShow(true)"
          >&times;</span
        >
        <div class="w-full h-full flex flex-col justify-center items-center">
          <input
            type="text"
            placeholder="Search..."
            name="search"
            class="w-96 px-3 py-2 border-0 focus:outline-0 mb-2"
            v-model="searchText"
          />
          <transition name="no-item-opacity">
            <!-- <div class="" v-if="isLoading">Loading...</div> -->
            <div
              class="bg-white w-96 px-3 py-2 border-0 text-center"
              v-if="filteredData.length == 0"
            >
              Oops, Tidak Ditemukan
            </div>
          </transition>
          <transition-group tag="div" name="no-item-opacity">
            <div
              class="bg-white w-96 px-3 py-2 border-0 text-center mb-2"
              v-for="(item, index) in filteredData"
              :key="index"
            >
              {{ item.name }}
            </div>
          </transition-group>
        </div>
      </div>
    </transition>
  </div>
</template>
