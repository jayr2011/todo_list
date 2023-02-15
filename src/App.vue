<script setup>
/* eslint-disable */
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

const inputContentText = ref("");
const inputCategoryRadio = ref(null);

const todosAsync = computed(() =>
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

const addTodo = () => {
  if (inputContentText.value.trim() === "" || inputContentText.value === null) {
    return;
  } else {
    todos.value.push({
      content: inputContentText.value,
      category: inputCategoryRadio.value,
      done: false,
      createdAt: new Date().getTime(),
    });
    inputContentText.value = "";
    inputCategoryRadio.value = null;
  }
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem(todos, JSON.stringify(newVal));
  },
  { deep: true }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>
<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        como vai
        <input type="text" placeholder="Seu nome aqui" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>Crie suas Tarefas</h3>
      <form @submit.prevent="addTodo">
        <h4>O que você quer adicionar a sua lista de tarefas ?</h4>
        <input
          type="text"
          placeholder="Crie uma tarefa"
          v-model="inputContentText"
        />
        <h4>Por favor, Escolha uma categoria</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="caterogy"
              value="business"
              v-model="inputCategoryRadio"
            />
            <span class="bubble business"></span>
            <div>Trabalho</div>
          </label>
          <label>
            <input
              type="radio"
              name="caterogy"
              value="personal"
              v-model="inputCategoryRadio"
            />
            <span class="bubble personal"></span>
            <div>Pessoal</div>
          </label>
        </div>
        <input type="submit" value="Adicionar Tarefa" />
      </form>
    </section>
    <section v-if="todos.length > 0" class="todo-list">
      <h3>Tarefas</h3>
      <div class="list">
        <div
          v-for="todo in todosAsync"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Deletar</button>
          </div>
        </div>
      </div>
    </section>
    <div v-else class="todo-list__empty">
      <span> Que pena, você ainda não tem tarefas :( </span>
    </div>
  </main>
</template>

<style>
.todo-list__empty {
  text-align: center;
}
</style>
