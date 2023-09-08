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
    return b.createdAt - a.createdAt;
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
  userTitle.value = "";
  userCategory.value = null;
};

const removeTask = (task) => {
  tasks.value = tasks.value.filter((t) => t !== task);
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

onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem("tasks")) || [];
});
// set it so it stays on refresh??
</script>

<template>
  <header>
    <h1 class="website-title">TaskMaster</h1>
  </header>
  <main class="app">
    <!-- <section class="greeting">
      <h2 class="title">
        Hello
        <input
          class="user-input-name"
          type="text"
          placeholder="Enter name here"
          v-model="name"
        />
      </h2>
    </section> -->

    <section class="create-container">
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
            <div class="easy">easy</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="medium"
              v-model="userCategory"
            />
            <div class="bubble medium"></div>
            <div class="medium">medium</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="hard"
              v-model="userCategory"
            />
            <div class="bubble hard"></div>
            <div class="hard">hard</div>
          </label>
        </div>
        <input type="submit" value="Add Task" />
      </form>
    </section>
    <section class="task-list-container">
      <h3>Task List</h3>
      <h5 v-if="tasks.length === 0">No Tasks Yet -- Add Some Above !!</h5>
      <div class="list">
        <div
          v-for="task in tasksOrder"
          :key="task.id"
          :class="`task-item ${task.done && 'done'}`"
        >
          <label class="list-label">
            <input type="checkbox" v-model="task.done" />
            <div :class="`${task.category}`"></div>
            <div :class="`${task.category}`">{{ task.category }}</div>
          </label>
          <div class="content">
            <input type="text" v-model="task.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTask(task)">Delete</button>
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
.create-container {
  background-color: rgba(255, 255, 255, 0.5);
  width: 80%;
  box-shadow: 8px 4px 4px grey;
  display: flex;
  margin-top: 20px;
  flex-direction: column;
  align-items: center;
  border-radius: 20px;
  text-align: center;
  padding: 10px;
}
label {
  margin: 20px;
}

.title {
  background: none;
  border: none;
  font-size: 25px;
  text-align: center;
  display: flex;
  align-items: center;
}

.task-list-container {
  margin: 20px;
}

h4 {
  font-size: 20px;
}

h3 {
  font-size: 25px;
  text-align: center;
  text-decoration: underline;
}

input[type="text"] {
  padding: 20px;
  font-size: 15px;
  border-radius: 10px;
}

input[type="checkbox"] {
  height: 20px;
  width: 20px;
  outline: 2px solid rgb(0, 0, 0);
  margin-right: 15px;
  cursor: pointer;
}

input[type="submit"],
.delete {
  font-size: 20px;
  background-color: #ffffff;
  border: double 3px black;
  padding: 10px;
    cursor: pointer;
}

input[type="submit"] {
  margin: 20px;
}

.list-label {
  display: flex;
}

.task-item {
  display: flex;
  align-items: center;
  border-radius: 10px;
  justify-content: space-around;
  border: double 3px black;
  width: 1000px;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.559);
  margin: 10px;
}

.easy {
  color: green;
  font-size: 20px;
}
.medium {
  color: rgb(216, 109, 15);
  font-size: 20px;
}
.hard {
  color: rgb(216, 35, 19);
  font-size: 20px;
}
.difficulty-options {
  display: flex;
  justify-content: center;
}

h1 {
  margin-bottom: 0px;
  font-size: 50px;
  border-bottom: 2px solid black;
}
main {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input[type="radio"] {
  appearance: none;
  padding: 10px;
  background-color: #fff;
  margin: 0;
  border: 2px solid black;
  border-radius: 50%;
  transform: translateY(-0.075em);
  cursor: pointer;
}
input[type="radio"]:checked {
  background-color: rgb(12, 67, 12);
}

h5 {
  font-size: 20px;
  font-style:oblique;
  color:rgb(34, 30, 33)
}

.user-input-name {
  background: none;
  border: none;
  font-size: 25px;
  text-align: center;
  width: auto;
  min-width: 1em;
}
</style>
