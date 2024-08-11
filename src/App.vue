<template>
<main class="flex m-auto justify-center items-center h-dvh">
  <div class="flex-col border-2 p-4 rounded-2xl">
    <p class="text-gray-100 text-2xl font-bold text-center mb-4">Todo List</p>
    <section>
      <p class="text-gray-100">
        What's up, <input type="text" placeholder="Name here" v-model="name" class="p-2 text-gray-100 border-b bg-transparent">
      </p>
    </section>
  </div>
</main>
</template>

<script setup lang="ts">
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref('')

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name')
})
</script>