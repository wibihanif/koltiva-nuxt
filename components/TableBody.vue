<script setup>
let { body: transportations } = await queryContent("/transportation").findOne();

const tableState = ref(false);
let filteredTransportations = ref([]);

const showDefaultTable = () => {
  tableState.value = !tableState.value;
};

const showFilteredTable = () => {
  let arrNopol = [];
  const hashNopol = {};

  for (const transportation of transportations) {
    arrNopol.push(transportation.nopol);
  }

  for (const nopol of arrNopol) {
    if (!hashNopol[nopol]) {
      hashNopol[nopol] = 1;
    } else {
      hashNopol[nopol] += 1;
    }
  }

  let mostCommonNumber;
  let maxCount = 0;

  for (const number in hashNopol) {
    if (hashNopol[number] > maxCount) {
      mostCommonNumber = number;
      maxCount = hashNopol[number];
    }
  }

  filteredTransportations.value = transportations.filter((transportation) => {
    return transportation.nopol === mostCommonNumber;
  });

  tableState.value = !tableState.value;
};
</script>

<template>
  <template v-if="tableState">
    <tr
      class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700"
      v-for="(transportation, index) in filteredTransportations"
      :key="index"
    >
      <th
        scope="row"
        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
      >
        {{ transportation.nopol }}
      </th>
      <td class="px-6 py-4">{{ transportation.region }}</td>
    </tr>
  </template>

  <template v-else>
    <!-- Show the default transportations -->
    <tr
      class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700"
      v-for="(transportation, index) in transportations"
      :key="index"
    >
      <th
        scope="row"
        class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
      >
        {{ transportation.nopol }}
      </th>
      <td class="px-6 py-4">{{ transportation.region }}</td>
    </tr>
  </template>

  <div>
    <button
      @click="showDefaultTable"
      v-if="tableState"
      class="bg-blue-400 text-white mr-4 w-[170%] py-4"
    >
      Back to Default Table
    </button>

    <button
      @click="showFilteredTable"
      v-else
      class="bg-blue-400 text-white mr-4 w-[167%] py-4 text-center"
    >
      Filter Table With Most Same Nopol
    </button>
  </div>
</template>
