<template>
  <div class="p-5 flex">
    <!-- Left Column for Multiple Select Dropdown -->
    <div class="flex-1">
      <Dropdown :items="dropdownItems" :allowMultiple="true" @update:selected="updateSelectedMultiple" />
      <div v-if="selectedDataMultiple.length">
        <p>Selected Items (Multiple):</p>
        <ul>
          <li v-for="data in selectedDataMultiple" :key="data">{{ data }}</li>
        </ul>
      </div>
    </div>

    <!-- Right Column for Single Select Dropdown -->
    <div class="flex-1">
      <Dropdown :items="dropdownItems" :allowMultiple="false" @update:selected="updateSelectedSingle" />
      <div v-if="selectedDataSingle">
        <p>Selected Item (Single):</p>
        <p>{{ selectedDataSingle }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Dropdown from './components/DropDown.vue';

const dropdownItems = ref([
  { title: 'Option 1', value: '1' },
  { title: 'Option 2', value: '2' },
  { title: 'Option 3', value: '3' }
]);

const selectedDataMultiple = ref([]);
const selectedDataSingle = ref('');

const updateSelectedMultiple = (data) => {
  selectedDataMultiple.value = data;
};

const updateSelectedSingle = (data) => {
  selectedDataSingle.value = data.length > 0 ? data[0] : '';
};
</script>

<style scoped>
/* Add any additional styling if needed */
</style>
