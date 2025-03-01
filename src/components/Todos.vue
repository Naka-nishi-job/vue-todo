<script setup lang="ts">
import { ref } from "vue";
import AddTodoModal from "./AddTodoModal.vue";

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

// 子から受け取ったデータを表示する
const catchValue = (data: string) => {
  todos.value.push({
    id: todos.value.length + 1,
    content: data,
  });
};
</script>

<template>
  <main class="main">
    <h1 class="title">Todos</h1>
    <button @click="addTodo" class="button blue addButton">新規追加</button>

    <AddTodoModal v-model:isAdd="isAdd" @text-send-event="catchValue" />

    <div v-if="editId" class="overlay" @click.self="cancel">
      <div class="modal">
        <h2>編集画面</h2>
        <div>
          <p>Content:</p>
          <input type="text" v-model="editTargetValue" />
        </div>

        <div class="modal-buttons">
          <button @click="save" class="button blue">保存</button>
          <button @click="cancel" class="button gray">キャンセル</button>
        </div>
      </div>
    </div>

    <!-- <div v-if="isAdd" class="overlay" @click.self="cancelAdd">
      <div class="modal">
        <h2>追加画面</h2>
        <div>
          <p>Content:</p>
          <input type="text" v-model="addTargetValue" />
        </div>

        <div class="modal-buttons">
          <button @click="add" class="button blue">追加</button>
          <button @click="cancelAdd" class="button gray">キャンセル</button>
        </div>
      </div>
    </div> -->

    <table class="table" border="1">
      <tbody>
        <tr v-for="todo in todos" :key="todo.id">
          <td>{{ todo.content }}</td>
          <td>
            <button @click="editTodo(todo.id)" class="button blue">編集</button>
          </td>
          <td>
            <button @click="deleteTodo(todo.id)" class="button red">
              削除
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </main>
</template>

<style scoped>
.main {
  position: relative;
  width: 100%;
  max-width: 1000px;
}

.title {
  margin-top: 2rem;
  font-size: 1.6rem;
}

.button {
  padding: 0.3rem 1.6rem;
  border-radius: 6px;
  transition: 0.4s ease;
  cursor: pointer;
}

.blue {
  border: 1px solid var(--color-blue);
  background-color: var(--color-white);
  color: var(--color-blue);
}

.blue:hover {
  background-color: var(--color-blue);
  color: var(--color-white);
}

.red {
  border: 1px solid var(--color-red);
  background-color: var(--color-white);
  color: var(--color-red);
}

.gray {
  background-color: var(--color-white);
  border: 1px solid var(--color-gray);
  color: var(--color-gray);
}

.addButton {
  position: absolute;
  top: 0;
  right: 0;
}

.table {
  border-collapse: collapse;
  margin-top: 1rem;
}

.table tbody tr > td {
  width: 300px;
  text-align: center;
  padding: 1rem;
}

.overlay {
  position: fixed;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
  background-color: var(--color-pink);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: #fff;
  padding: 4rem;
}

.modal-buttons {
  margin-top: 2rem;
  display: flex;
  gap: 2rem;
  justify-content: center;
}
</style>
