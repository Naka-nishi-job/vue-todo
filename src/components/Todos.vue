<script setup lang="ts">
import { ref } from "vue";

// TodoのList(Refで監視済)
const todos = ref([
  { id: 1, content: "aaa" },
  { id: 2, content: "bbb" },
  { id: 3, content: "ccc" },
]);

// 編集するID, 値
const editId = ref(0);
const editTargetValue = ref("");

// 追加用
const isAdd = ref(false);
const addTargetValue = ref("");

/** 指定した要素を削除 */
const deleteTodo = (id: number) => {
  const data = todos.value.find((todo) => todo.id === id);
  if (!data) return;

  if (confirm(`${data.content}を削除してよろしいですか？`)) {
    todos.value = todos.value.filter((todo) => todo.id !== id);
  }
};

/** 指定した要素を編集 */
const editTodo = (id: number) => {
  // TodoをIDで検索
  const todo = todos.value.find((todo) => {
    return todo.id === id;
  });
  if (!todo) return;

  // 編集中のIDを更新
  editId.value = id;

  // 編集用画面へ値を渡す
  editTargetValue.value = todo.content;
};

// キャンセル処理
const cancel = () => {
  // 初期化処理
  editId.value = 0;
  editTargetValue.value = "";
};

// 追加キャンセル処理
const cancelAdd = () => {
  isAdd.value = false;
  addTargetValue.value = "";
};

/** 編集内容を保存する */
const save = () => {
  // 更新対象のTodoを取得
  const todo = todos.value.find((todo) => todo.id === editId.value);
  if (todo) {
    // Todoの更新（一部を更新してもOK）
    todo.content = editTargetValue.value;
  }

  cancel();
};

/** Todoの追加 */
const addTodo = () => {
  isAdd.value = true;
};

/** 追加処理 */
const add = () => {
  todos.value.push({
    id: todos.value.length + 1,
    content: addTargetValue.value,
  });

  // 初期化処理
  isAdd.value = false;
  addTargetValue.value = "";
};
</script>

<template>
  <h1>Todos</h1>

  <div v-if="editId" class="overlay" @click.self="cancel">
    <div class="modal">
      <h2>編集画面</h2>
      <div>
        <p>Content:</p>
        <input type="text" v-model="editTargetValue" />
      </div>

      <div class="modal-buttons">
        <button @click="save">保存</button>
        <button @click="cancel">キャンセル</button>
      </div>
    </div>
  </div>

  <div v-if="isAdd" class="overlay" @click.self="cancelAdd">
    <div class="modal">
      <h2>追加画面</h2>
      <div>
        <p>Content:</p>
        <input type="text" v-model="addTargetValue" />
      </div>

      <div class="modal-buttons">
        <button @click="add">追加</button>
        <button @click="cancelAdd">キャンセル</button>
      </div>
    </div>
  </div>

  <div>
    <button @click="addTodo">新規追加</button>
  </div>

  <table>
    <tbody>
      <tr v-for="todo in todos" :key="todo.id">
        <td>{{ todo.content }}</td>
        <td><button @click="editTodo(todo.id)">編集</button></td>
        <td><button @click="deleteTodo(todo.id)">削除</button></td>
      </tr>
    </tbody>
  </table>
</template>

<style>
.overlay {
  background-color: pink;
  position: absolute;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: #fff;
}

.modal-buttons {
  margin-top: 2rem;
  display: flex;
  gap: 2rem;
  justify-content: center;
}
</style>
