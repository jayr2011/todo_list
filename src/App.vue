<script setup>
/* eslint-disable */
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

const inputContentText = ref("");
const inputCategoryRadio = ref(null);

const todosAsync = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (inputContentText.value.trim() === "" || inputContentText.value === null) {
    return;
  } else {
    todos.value.push({
      content: inputContentText.value,
      category: inputCategoryRadio,
      done: false,
      createdAt: new Date().getTime(),
    });
  }
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
});
</script>
<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Whats up,
        <input type="text" placeholder="name here" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>create a todo</h3>
      <form @submit.prevent="addTodo">
        <h4>Whats on your todo list ?</h4>
        <input
          type="text"
          placeholder="e.g make a video"
          v-model="inputContentText"
        />
        <h4>pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="caterogy"
              value="business"
              v-model="inputCategoryRadio"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input
              type="radio"
              name="caterogy"
              value="personal"
              v-model="inputCategoryRadio"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" />
      </form>
    </section>
  </main>
</template>
