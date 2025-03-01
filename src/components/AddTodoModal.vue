<script setup lang="ts">
import { ref } from "vue";

// このコンポーネントで受け取るものを定義する
const props = defineProps<{
  isAdd: boolean;
}>();

// 入力値を監視
const text = ref("");
const changeText = (value: string) => {
  console.log(value);
};

// 親へ渡す名前と内容を定義する
// text-send-eventという名前で、string型のtextを親へ送信する
const emit = defineEmits<{
  (event: "text-send-event", textValue: string): void;
  (event: "update:isAdd", isAdd: boolean): void;
}>();

const addTodo = () => {
  emit("text-send-event", text.value);
  cancelAdd();
};

const cancelAdd = () => {
  text.value = "";
  emit("update:isAdd", false);
};
</script>

<template>
  <div v-if="props.isAdd" class="overlay" @click.self="cancelAdd">
    <div class="modal">
      <h2>追加画面</h2>
      <div>
        <p>Content:</p>
        <input type="text" v-model="text" @change="changeText(text)" />
      </div>

      <div class="modal-buttons">
        <button @click="addTodo" class="button blue">追加</button>
        <button @click="cancelAdd" class="button gray">キャンセル</button>
      </div>
    </div>
  </div>
</template>
