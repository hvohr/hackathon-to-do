<script setup>
import { ref, onMounted, computed, watch } from 'vue'
// ref = handling state
// mounted = when we first render page/component
// computed = things in order
// watch = watch for changes in ref/updates local storage
const tasks = ref([])
const name = ref('')
const userTitle = ref('')
const userCategory = ref(null)
</script>

<template>

</template>

<style scoped>

</style>
