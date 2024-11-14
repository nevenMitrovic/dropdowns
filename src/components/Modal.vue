<template>
  <div
    v-show="isOpen"
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
  >
    <div
      class="bg-white rounded-lg shadow-lg p-6 max-w-sm w-2/3 sm:w-full text-center"
    >
      <div class="flex justify-center items-center mb-4"></div>
      <h2 class="text-base sm:text-xl text-black font-semibold mb-2">Error</h2>
      <p class="text-gray-500 mb-4 text-xs sm:text-base">
        {{ error }}
      </p>
      <div class="flex items-center justify-center">
        <button
          @click="closeModal"
          class="px-4 py-2 bg-red-500 text-white rounded-lg hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-400"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  error: String,
  modalVisibility: Boolean,
});

const isOpen = ref(props.modalVisibility);

const closeModal = () => {
  isOpen.value = false;
};

watch(
  () => props.modalVisibility,
  (newVal) => {
    isOpen.value = newVal;
  }
);
</script>
