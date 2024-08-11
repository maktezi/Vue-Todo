<template>
  <main class="flex m-auto justify-center items-center h-dvh">
    <div class="flex-col max-w-xl border p-8 rounded-2xl">
      <section class="mb-4">
        <p class="text-lg w-full">
          What's up,
          <input
            type="text"
            placeholder="Name here"
            v-model="name"
            class="font-bold px-2 outline-none border-none bg-transparent"
          />
        </p>
      </section>

      <section>
        <p class="mb-4 rounded-xl font-bold cursor-default">
          CREATE A TODO LIST
        </p>
        <form @submit.prevent="addTodo">
          <p class="text-sm mb-1 cursor-default">What's on your mind?</p>
          <input
            class="mb-4 pl-4 py-3 text-sm w-full rounded-full bg-gray-600"
            type="text"
            placeholder="e.g. buy foods"
            v-model="input_content"
          />

          <p class="text-sm mb-1 cursor-default">Category</p>
          <div class="flex justify-evenly flex-grow">
            <label
              class="cursor-pointer transition border-none px-14 py-6 rounded-2xl bg-blue-900/50 hover:bg-blue-900"
            >
              <input
                class="mr-2 w-full text-center flex"
                type="radio"
                name="category"
                id="category1"
                value="work"
                v-model="input_category"
              />
              <span>Work</span>
            </label>

            <label
              class="cursor-pointer transition border-none px-10 py-6 rounded-2xl bg-yellow-900/50 hover:bg-red-900"
            >
              <input
                class="mr-2 w-full text-center flex"
                type="radio"
                name="category"
                id="category2"
                value="personal"
                v-model="input_category"
              />
              <span>Personal</span>
            </label>
          </div>

          <input
            type="submit"
            value="Add todo"
            class="w-full transition bg-green-900/50 hover:bg-green-900 cursor-pointer bg-gray-500 mt-6 p-3 rounded-full"
          />
        </form>
      </section>

      <section class="mt-8">
        <p class="mb-2 rounded-xl font-bold cursor-default">TODO LIST</p>
        <div class="max-h-96 overflow-y-auto pr-2">
          <div
            v-for="todo in todos_asc"
            class="hover:bg-green-900/50 transition"
            :class="[
              'todo-item flex p-0.5 rounded-full items-center justify-between mb-1',
              { done: todo.done },
              todo.category === '' ? 'bg-gray-500/10' : '',
              todo.category === 'work' ? 'bg-blue-900/50' : '',
              todo.category === 'personal' ? 'bg-yellow-900/50' : '',
            ]"
          >
            <div class="flex pl-2 items-center">
              <label>
                <input type="checkbox" v-model="todo.done" />
              </label>

              <div class="w-[310px]">
                <input
                  class="ml-2 w-full text-sm bg-transparent outline-none"
                  type="text"
                  v-model="todo.content"
                  :class="`todo-${todo.category}`"
                />
              </div>
            </div>

            <div>
              <button
                class="bg-red-900/50 p-1.5 px-3 rounded-full text-sm"
                @click="removeTodo(todo)"
              >
                X
              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref, onMounted, computed, watch } from "vue";

const todos: any = ref([]);

const name = ref("");
const input_content = ref("");
const input_category = ref("");

const todos_asc = computed(() =>
  todos.value.sort((a: any, b: any) => {
    return b.createdAt - a.createdAt;
  }),
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null)
    return;

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_content.value = "";
  input_category.value = "";
};

const removeTodo = (todo: any) => {
  const isConfirmed = confirm("Are you sure you want to remove this todo?");
  if (isConfirmed) {
    todos.value = todos.value.filter((t: any) => t !== todo);
  }
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true },
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>
