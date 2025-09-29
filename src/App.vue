<template>
  <div>
    <h1>Vue 3 高性能可编辑列表</h1>

    <div class="controls">
      <button @click="addItem">添加新项目</button>
      <span>总数: {{ totalItems }}</span>
    </div>

    <ul class="item-list">
      <ListItem
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :text="item.text"
        :update-time="item.updateTime"
        @update="updateItemText"
      />
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import ListItem from "./components/ListItem.vue";

const items = ref(
  Array.from({ length: 5 }, (_, i) => ({
    id: i + 1,
    text: `项目 ${i + 1}`,
    updateTime: null,
  }))
);

let nextId = 6;

const totalItems = computed(() => {
  console.log("计算属性: totalItems 被重新计算");
  return items.value.length;
});

const addItem = () => {
  items.value.unshift({
    id: nextId++,
    text: `新项目 ${nextId - 1}`,
    updateTime: null,
  });
};

const updateItemText = (id, newText) => {
  const item = items.value.find((i) => i.id === id);
  if (item) {
    item.text = newText;
    // 强制整个列表更新以显示时间
    items.value = [...items.value];
  }
};
</script>
