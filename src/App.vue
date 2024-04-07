<template>
  <v-app>
    <MyBar />
    <AppFooter />

    <!-- 侧边栏 -->
      <MyDrawer :updateTableData="updateTableData" @selected-change="handleSelectionChange" />

      <!-- 主内容区域 -->
      <v-main>
        <!-- DataTable2 组件将填充剩余的空间 -->
        <MyDataTable :currentTableData="currentTableData" />
      </v-main>

  </v-app>
</template>

<script setup>
import { ref, reactive } from 'vue'

const vegetables = ref([
    {
      name: 'Spinach',
      calories: 23,
      fat: 0.4,
      carbs: 3.6,
      protein: 2.9,
      iron: '15%',
      select: false
    },
    {
      name: 'Kael',
      calories: 49,
      fat: 0.9,
      carbs: 8.8,
      protein: 4.3,
      iron: '16%',
      select: false
    },
    {
      name: 'Broccoli',
      calories: 34,
      fat: 0.4,
      carbs: 6.6,
      protein: 2.8,
      iron: '6%',
      select: false
    },
    {
      name: 'Brussels Sprouts',
      calories: 43,
      fat: 0.3,
      carbs: 8.9,
      protein: 3.4,
      iron: '9%',
      select: false
    },
    {
      name: 'Avocado',
      calories: 160,
      fat: 15,
      carbs: 9,
      protein: 2,
      iron: '3%',
      select: false
    },
    {
      name: 'Sweet Potato',
      calories: 86,
      fat: 0.1,
      carbs: 20.1,
      protein: 1.6,
      iron: '3%',
      select: false
    },
    {
      name: 'Corn',
      calories: 96,
      fat: 1.5,
      carbs: 21,
      protein: 3.4,
      iron: '2%',
      select: false
    },
    {
      name: 'Potato',
      calories: 77,
      fat: 0.1,
      carbs: 17.5,
      protein: 2,
      iron: '8%',
      select: false
    },
    {
      name: 'Butternut Squash',
      calories: 45,
      fat: 0.1,
      carbs: 11.7,
      protein: 1,
      iron: '4%',
      select: false
    },
    {
      name: 'Beetroot',
      calories: 43,
      fat: 0.2,
      carbs: 10,
      protein: 1.6,
      iron: '6%',
      select: false
    },
    {
      name: 'Parsnip',
      calories: 75,
      fat: 0.3,
      carbs: 18,
      protein: 1.2,
      iron: '4%',
      select: false
    },
    {
      name: 'Yam',
      calories: 118,
      fat: 0.2,
      carbs: 27.9,
      protein: 1.5,
      iron: '4%',
      select: false
    },
    {
      name: 'Acorn Squash',
      calories: 40,
      fat: 0.1,
      carbs: 10,
      protein: 1,
      iron: '5%',
      select: false
    },
    {
      name: 'Artichoke',
      calories: 47,
      fat: 0.2,
      carbs: 10.5,
      protein: 3.3,
      iron: '7%',
      select: false
    },
    {
      name: 'Peas',
      calories: 81,
      fat: 0.4,
      carbs: 14.5,
      protein: 5.4,
      iron: '25%',
      select: false
    },
    {
      name: 'Green Beans',
      calories: 31,
      fat: 0.1,
      carbs: 6.9,
      protein: 1.8,
      iron: '8%',
      select: false
    },
    {
      name: 'Red Bell Pepper',
      calories: 26,
      fat: 0.2,
      carbs: 6.0,
      protein: 1.0,
      iron: '3%',
      select: false
    },
    {
      name: 'Cauliflower',
      calories: 25,
      fat: 0.1,
      carbs: 5.0,
      protein: 1.9,
      iron: '4%',
      select: false
    },
    {
      name: 'Zucchini',
      calories: 17,
      fat: 0.3,
      carbs: 3.1,
      protein: 1.2,
      iron: '3%',
      select: false
    },
    {
      name: 'Asparagus',
      calories: 20,
      fat: 0.1,
      carbs: 3.9,
      protein: 2.2,
      iron: '16%',
      select: false
    },
    {
      name: 'Eggplant',
      calories: 25,
      fat: 0.2,
      carbs: 6,
      protein: 1,
      iron: '1%',
      select: false
    },
    {
      name: 'Pumpkin',
      calories: 26,
      fat: 0.1,
      carbs: 6.5,
      protein: 1,
      iron: '4%',
      select: false
    },
    {
      name: 'Celery',
      calories: 16,
      fat: 0.2,
      carbs: 3,
      protein: 0.7,
      iron: '1%',
      select: false
    },
    {
      name: 'Cucumber',
      calories: 15,
      fat: 0.1,
      carbs: 3.6,
      protein: 0.7,
      iron: '2%',
      select: false
    },
    {
      name: 'Leek',
      calories: 61,
      fat: 0.3,
      carbs: 14.2,
      protein: 1.5,
      iron: '12%',
      select: false
    },
    {
      name: 'Fennel',
      calories: 31,
      fat: 0.2,
      carbs: 7,
      protein: 1.2,
      iron: '6%',
      select: false
    },
    {
      name: 'Green Peas',
      calories: 81,
      fat: 0.4,
      carbs: 14.5,
      protein: 5.4,
      iron: '25%',
      select: false
    },
    {
      name: 'Okra',
      calories: 33,
      fat: 0.2,
      carbs: 7.5,
      protein: 1.9,
      iron: '3%',
      select: false
    },
    {
      name: 'Chard',
      calories: 19,
      fat: 0.2,
      carbs: 3.7,
      protein: 1.8,
      iron: '22%',
      select: false
    },
    {
      name: 'Collard Greens',
      calories: 32,
      fat: 0.6,
      carbs: 5.4,
      protein: 3,
      iron: '2%',
      select: false
    },
  ]);

// const data = reactive({
//    currentTableData: []
// });

const select_ids = ref([]);

const data = ref(1)

const currentTableData = reactive(
  {
    value: [],
    allSelected: false
  }
);

const currentIndex = ref(0);

function updateTableData(index) {
  currentTableData.value = vegetables.value.slice(index*index*4, index*index*4 + 4 + index*4);
  currentIndex.value = index;
  if (select_ids.value.includes(index)) {
    currentTableData.allSelected = true;
  } else {
    currentTableData.allSelected = false;
  }
  console.log("app.vue", currentTableData.value);
  console.log("allSelected.value", currentTableData.allSelected);
}

function handleSelectionChange(selected_ids) {
  select_ids.value = selected_ids;
  console.log("Selected ids: ", select_ids.value);
  selected_ids.forEach(index => {
    vegetables.value.slice(index * index * 4, index * index * 4 + 4 + index * 4).forEach(item => {
      item.select = true;
    });
  });
  updateTableData(currentIndex.value);
}

</script>