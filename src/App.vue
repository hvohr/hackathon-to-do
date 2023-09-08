<script setup>
import { ref, onMounted, computed, watch } from "vue";
// ref = handling state
// mounted = when we first render page/component
// computed = things in order
// watch = watch for changes in ref/updates local storage
// v-model = implement a two-way binding for input elements??
// v-for = a directive which is used to loop over data that is usually an array or object
// using : for an attribute has a binding agent??

const tasks = ref([]);
const name = ref("");
const userTitle = ref("");
const userCategory = ref(null);
// const emptyError = ref(false)

const tasksOrder = computed(() =>
  tasks.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);
// ascending list by created time??

const addTask = () => {
  if (userTitle.value.trim() === "" || userCategory.value === null) {
    return;
  }
  tasks.value.push({
    content: userTitle.value,
    category: userCategory.value,
    done: false,
    id: Date.now(),
    createdAt: new Date().getTime(),
  });
};

watch(
  tasks,
  (newTask) => {
    localStorage.setItem("tasks", JSON.stringify(newTask));
  },
  { deep: true }
);

// deep looks through each induvidual array item/element
// and if anything changes, deep will catch it)

watch(name, (newNameValue) => {
  localStorage.setItem("name", newNameValue);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  tasks.value = JSON.parse(localStorage.getItem("tasks")) || [];
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
        <input type="submit" value="Add Task" />
      </form>
    </section>
    <section>
      <h3>Task List</h3>
      <div class="list">
        <div
          v-for="task in taskOrder"
          :key="task.id"
          :class="`task-item ${task.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="task.done" />
            <div :class="`bubble ${task.category}`"></div>
          </label>
          <div class='content'>
            <input type='text' v-model="task.content"/>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
header {
  text-align: center;
}

input[type="radio"] {
  appearance: none;
  padding: 10px;
  background-color: #fff;
  margin: 0;
  border: 2px solid black;
  border-radius: 50%;
  transform: translateY(-0.075em);
}
input[type="radio"]:checked {
  background-color: rgb(12, 67, 12);
}
</style>
