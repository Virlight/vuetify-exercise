<template>
  <v-container>
    <v-row class="fill-height">
      <v-data-table
        v-model="selected"
        :headers="headers"
        :items="vegetables"
        item-value="name"
        show-select
      >          
      <!-- <template v-slot:item.name="{ value }">
        <v-checkbox
          v-model="selected"
          :value="value"
          :label="value"
          hide-details
        ></v-checkbox>
      </template> -->

        <template v-slot:item.calories="{ value }">
          <v-chip :color="getColor(value)">
            {{ value }}
          </v-chip>
        </template>
      </v-data-table>
    </v-row>
  </v-container>
</template>   

<style scoped>
/* CSS 来确保 v-container 和 v-row 填满除了页脚和头部导航栏的全部高度, 100vh代表100%在高度上填充页面, 但是还包括了top bar和footer, 所以要减去, 同理100vw代表100%宽度填充 
  一定要设置min-height, 才能满足在即使没有数据的时候也能扩展页面, 光设置height不够 */
.fill-height {
  min-height: calc(100vh - 90px);
}
</style>

<script setup>
  import {ref, watch, watchEffect, computed} from 'vue'

  const props = defineProps({
    currentTableData: Object
  });

  const vegetables = computed(() => props.currentTableData.value);
  const allSelected = computed(() => props.currentTableData.allSelected);

  const selected = ref([]);

  watchEffect(() => {
  // 清空当前选中项
  selected.value = [];
  
  // 遍历 vegetables 数组
  vegetables.value.forEach(item => {
    if (item.selected) {
      // 如果项被选中，则将其名称添加到 selected 数组中
      selected.value.push(item.name);
    }
  });
  
  console.log("MyDataTable.vue, watching started...");
  console.log("vagetables: ", vegetables.value);
  console.log("MyDataTable.vue, watching ended...");
});

  watch(() =>  selected.value, (newValue, oldVal) => {
    console.log("MyDataTable.vue, selected changed to: ", newValue);
  });

  // 监听 allSelected 的变化
  watch(() => allSelected.value, (newValue, oldVal) => {
    if (newValue) {
      // 如果 allSelected 为 true，将所有可选项的 name 属性添加到 selected 数组中
      selected.value = vegetables.value.map(item => item.name);
    } else {
      // 如果 allSelected 为 false，清空 selected 数组
      selected.value = [];
    }
    console.log("MyDataTable.vue, allSelected changed to: ", newValue);
  }, { immediate: true });

  const headers = [
    { title: 'Vegetable (100g serving)', key: 'name' },
    { title: 'Calories', key: 'calories' },
    { title: 'Fat (g)', key: 'fat' },
    { title: 'Carbs (g)', key: 'carbs' },
    { title: 'Protein (g)', key: 'protein' },
    { title: 'Iron (%)', key: 'iron' },
  ]

  function getColor (calories) {
    if (calories > 100) return 'red'
    else if (calories > 50) return 'orange'
    else return 'green'
  }
</script>
  