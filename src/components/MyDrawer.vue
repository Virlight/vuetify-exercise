<template>
  <v-row no-gutters>
    <v-col cols="auto">
      <v-navigation-drawer
        theme="dark"
        permanent
        rail
        expand-on-hover
        width="200"
      >
        <v-list-item
          prepend-avatar="https://randomuser.me/api/portraits/women/75.jpg"
          nav
        ></v-list-item>

        <v-divider></v-divider>

        <v-list
          density="compact"
          nav
        >
          <v-list-item v-for="(chain, index) in chains" :prepend-icon="chain.icon" :key="index" :title="chain.title" :value="chain.value" @click="setDrawerContent(index)"></v-list-item>
        </v-list>
      </v-navigation-drawer>
    </v-col>
    <v-col cols="auto">

      <v-navigation-drawer permanent>
        <v-checkbox
          label="Select All"
          v-model="drawerContent.selectAll"
          @change="toggleAll"
        ></v-checkbox>

        <v-divider></v-divider>

        <v-list>
          <v-list-item v-for="(item, index) in drawerContent.items" :key="index" :title="item.title" :value="item.value" @click="handleClick(index)">
            <template v-slot:prepend>
              <v-list-item-action start>
                <v-checkbox-btn v-model="drawerContent.selected" :value="index" @click.stop="toggleSelection(item)"></v-checkbox-btn>
              </v-list-item-action>
            </template>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>

      <!-- 这样写, 也可以 -->
      <!-- <v-navigation-drawer permanent>
        <v-list>
          <v-list-item v-for="(item, index) in drawerContent.items" :key="index">
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer> -->
    </v-col>
  </v-row>
</template>



<script setup>
import { ref, reactive, watch, defineEmits, nextTick } from 'vue';

const props = defineProps({
  updateTableData: Function // 定义接收的 prop，类型为 Function
});

const emit = defineEmits(['selected-change']);

const drawerContent = reactive({
        items: [],
        selected: [],
        selectAll: false
    });

function setDrawerContent(index) {
  drawerContent.items = contents[index];
  drawerContent.selected = [];
  drawerContent.selectAll = false;
}

// 切换全选/全不选状态
function toggleAll() {
  drawerContent.items.forEach(item => {
    item.selected = drawerContent.selectAll;
  });
  if (drawerContent.selectAll) {
    // 如果全选，则添加所有项目的值到 selected 数组
    drawerContent.selected = drawerContent.items.map((item, index) => index);
  } else {
    // 如果全不选，则清空 selected 数组
    drawerContent.selected = [];
  }
  console.log("All items selected:", drawerContent.selectAll);
  console.log("Selected items:", drawerContent.selected);
}

// 检查是否所有项都被选中，来更新全选复选框的状态
function checkSelectAll() {
  const allSelected = drawerContent.items.every(item => item.selected);
  drawerContent.selectAll = allSelected;
  console.log("All items selected:", allSelected);
}

const handleClick = (index) => {
  console.log("Clicked item index:", index);
  try {
    props.updateTableData(index); 
  } catch (error) {
    console.error("Error updating table data:", error);
  }
}

const toggleSelection = (item) => {
  item.selected = !item.selected;
  console.log("is this item selected? ", item.selected);
  checkSelectAll()
};

watch(() => drawerContent.selected, (newVal, oldVal) => {
  emit('selected-change', drawerContent.selected);
  console.log("start watching selected items...");
  console.log("Selected items changed:", newVal);
  console.log("end watching selected items...");
});

const chains =   [
        { icon: "mdi-bitcoin", title: "Bitcoin", value: "bitcoin" },
        { icon: "mdi-ethereum", title: "Ethereum", value: "ethereum" },
        { icon: "mdi-currency-btc", title: "Others", value: "other" }
    ]

const contents = [
    [
    { title: "Home1", value: "home", selected: false },
    { title: "Contacts1", value: "contacts", selected: false },
    { title: "Settings1", value: "settings", selected: false }
    ],
    [
    { title: "Home2", value: "home", selected: false },
    { title: "Contacts2", value: "contacts", selected: false },
    { title: "Settings2", value: "settings", selected: false  }
    ],
    [
    { title: "Home3", value: "home",selected: false },
    { title: "Contacts3", value: "contacts", selected: false },
    { title: "Settings3", value: "settings", selected: false }
    ],
    // 可以添加更多的内容数组
];

</script>
    