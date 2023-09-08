<script setup>
import { ref, onMounted, computed, watch } from "vue";
// ref = handling state
// mounted = when we first render page/component
// computed = things in order
// watch = watch for changes in ref/updates local storage
// v-model = implement a two-way binding for input elements??

const tasks = ref([]);
const name = ref("");
const userTitle = ref("");
const userCategory = ref(null);

const tasksOrder = computed(() =>
  tasks.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);
// ascending list by created time??

const addTask = () => {
  // insert things
};

watch(name, (newNameValue) => {
  localStorage.setItem("name", newNameValue);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
});
// set it so it stays on refresh??
</script>

<template>
  <header>
    <h1 class="website-title">TaskMaster</h1>
  </header>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Hello <input type="text" placeholder="Enter name here" v-model="name" />
      </h2>
    </section>

    <section>
      <h3>Create a New Task</h3>
      <form @submit.prevent="addTask">
        <h4>What's on your task list?</h4>
        <input
          type="text"
          placeholder="e.g. walk the dog"
          v-model="userTitle"
        />
        <h4>What degree of difficulty is this task?</h4>
        <div class="difficulty-options">
          <label>
            <input
              type="radio"
              name="category"
              value="easy"
              v-model="userCategory"
            />
            <div class="bubble easy"></div>
            <div>Easy</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="medium"
              v-model="userCategory"
            />
            <div class="bubble medium"></div>
            <div>Medium</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="hard"
              v-model="userCategory"
            />
            <div class="bubble hard"></div>
            <div>Hard</div>
          </label>
        </div>
      </form>
    </section>
  </main>
</template>

<style scoped>
header {
  text-align: center;
}
/* input:not([type="radio"]):not([type="checkbox"]), button {
	appearance: none;
	border: none;
	outline: none;
	background: none;
	cursor: initial;
} */
</style>
