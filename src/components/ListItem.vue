<template>
  <li class="list-item">
    <input :value="text" @input="onInput" />
    <span>上次更新: {{ formattedTime }}</span>
  </li>
</template>

<script>
import { ref, watch } from "vue";

export default {
  props: ["id", "text", "updateTime"],
  emits: ["update"],
  setup(props, { emit }) {
    const lastUpdateTime = ref(props.updateTime);

    // 观察 text prop 的变化来更新时间
    watch(
      () => props.text,
      () => {
        lastUpdateTime.value = new Date();
      }
    );

    const onInput = (event) => {
      emit("update", props.id, event.target.value);
    };

    // 这是一个昂贵的计算，模拟复杂逻辑
    const calculateFormattedTime = (time) => {
      if (!time) return "尚未更新";
      console.log(`[昂贵计算] 正在为 item ${props.id} 格式化时间`);
      // 模拟耗时操作
      let i = 0;
      while (i < 10000000) {
        i++;
      }
      return time.toLocaleTimeString();
    };

    const formattedTime = calculateFormattedTime(lastUpdateTime.value);

    return {
      onInput,
      formattedTime,
    };
  },
};
</script>
