<template>
  <div class="w-full">
    <label class="block text-gray-700 font-medium mb-1">{{
      props.label
    }}</label>
    <select
      v-model="selectedOption"
      @change="onSelectChange"
      class="w-full px-4 py-2 border border-gray-300 rounded-lg shadow-md focus:outline-none focus:ring-2 focus:ring-blue-500"
    >
      <option
        v-for="(option, index) in props.options"
        :key="index"
        :value="option"
      >
        {{ option }}
      </option>
    </select>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";

const props = defineProps({
  label: {
    type: String,
  },
  options: {
    type: Array,
  },
});
const emit = defineEmits(["update:modelValue"]);
const selectedOption = ref(props.options[0]); // set initial selected item

const onSelectChange = () => {
  emit("update:modelValue", selectedOption);
};
</script>
