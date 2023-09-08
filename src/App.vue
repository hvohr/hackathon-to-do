<script setup>
import { ref, onMounted, computed, watch } from "vue";
// ref = handling state
// mounted = when we first render page/component
// computed = things in order
// watch = watch for changes in ref/updates local storage
// v-model = implement a two-way binding for input elements??

const tasks = ref([]);
const name = ref('')
const userTitle = ref("");
const userCategory = ref(null);

const tasksOrder = computed(() =>
  tasks.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);
// ascending list by created time??

watch(name, (newNameValue) => {
  localStorage.setItem('name', newNameValue)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})
// set it so it stays on refresh??

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Hello <input type='text' placeholder="Enter name here" v-model='name'/>
        
      </h2>
    </section>
  </main>
</template>

<style scoped>
/* input:not([type="radio"]):not([type="checkbox"]), button {
	appearance: none;
	border: none;
	outline: none;
	background: none;
	cursor: initial;
} */
</style>
