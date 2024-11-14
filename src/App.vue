<template>
  <div
    class="flex flex-col items-center justify-center min-h-screen p-4 bg-gray-100"
  >
    <h1 class="text-2xl font-bold mb-6 text-center">Nexus Dropdowns Test</h1>

    <form
      @submit.prevent="handleSubmit"
      class="w-full bg-white p-20 max-w-lg rounded-xl space-y-4 shadow-md"
    >
      <Dropdown
        label="Year"
        :options="yearOptions"
        v-model="selectedQueries.year"
        @change="loadMakes"
      />
      <Dropdown
        label="Make"
        :options="makeOptions"
        :isDisabled="isDisabledMake"
        v-model="selectedQueries.make"
        @change="loadModels"
      />
      <Dropdown
        label="Model"
        :options="modelOptions"
        :isDisabled="isDisabledModel"
        v-model="selectedQueries.model"
      />

      <button
        type="submit"
        :disabled="isDisabled"
        class="w-full px-4 py-2 text-white rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
        :class="
          isDisabled
            ? 'bg-gray-500 hover:bg-gray-600'
            : 'bg-blue-500 hover:bg-blue-600'
        "
      >
        Submit
      </button>
    </form>
    <Modal :error="errorMessage" :modal-visibility="modalVisibility" />
  </div>
</template>

<script setup>
import { onMounted, reactive, ref, computed } from "vue";
import Dropdown from "./components/Drodown.vue";
import Modal from "./components/Modal.vue";

const isDisabled = computed(() => {
  return (
    !selectedQueries.year || !selectedQueries.make || !selectedQueries.model
  );
});
const isDisabledMake = computed(() => {
  return !selectedQueries.year;
});
const isDisabledModel = computed(() => {
  return !selectedQueries.year || !selectedQueries.make;
});

const errorMessage = ref(null);
const modalVisibility = ref(false);
const yearOptions = ref(["Select an option"]);
const makeOptions = ref(["Select an option"]);
const modelOptions = ref(["Select an option"]);
const selectedQueries = reactive({
  year: null,
  make: null,
  model: null,
});

const handleSubmit = () => {
  console.log(selectedQueries);
};

const loadMakes = async () => {
  try {
    makeOptions.value = ["Select an option"];
    if (selectedQueries.year) {
      const res = await fetch(
        `api/v2/vehicles/makes/?year=${selectedQueries.year}&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`
      );
      if (!res.ok) throw new Error("Network response was not ok");

      const data = await res.json();
      data.forEach((item) => {
        makeOptions.value.push(item.make);
      });
    }
  } catch (error) {
    console.error(error);
    errorMessage.value = error.message;
    modalVisibility.value = true;
  }
};

const loadModels = async () => {
  try {
    modelOptions.value = ["Select an option"];
    if (selectedQueries.make) {
      const res = await fetch(
        `api/v2/vehicles/models/?year=${selectedQueries.year}&make=${selectedQueries.make}&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`
      );
      if (!res.ok) throw new Error("Network response was not ok");

      const data = await res.json();
      data.forEach((item) => {
        modelOptions.value.push(item.model);
      });
    }
  } catch (error) {
    console.error(error);
    errorMessage.value = error.message;
    modalVisibility.value = true;
  }
};

onMounted(async () => {
  try {
    const res = await fetch(
      `api/v2/vehicles/years/?token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`
    );
    if (!res.ok) throw new Error("Network response was not ok");

    const data = await res.json();
    data.forEach((item) => {
      yearOptions.value.push(item.year);
    });
  } catch (error) {
    console.error(error);
    errorMessage.value = error.message;
    modalVisibility.value = true;
  }
});
</script>
