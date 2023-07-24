<script setup lang="ts">
import { computed, ref } from 'vue';
import { Todo } from '../models/Todo';
import ShowTodos from './ShowTodos.vue';
import AddTodo from './AddTodo.vue';

const todos = ref<Todo[]>(JSON.parse(localStorage.getItem('todos') || '[]'));

const displayCompletedTodos = ref(false);

let colorIndex = 0;

const addTodo = (newTodo: string) => {
  const randomColor = getRandomColor();
  todos.value.push(new Todo(newTodo, false, randomColor));
  saveToLS(todos.value);
};

function getRandomColor(): string {
  const colors = ['#4285F4', '#DB4437', '#F4B400', '#0F9D58'];
  const color = colors[colorIndex];
  colorIndex = (colorIndex + 1) % colors.length;
  return color;
}

const deleteTodo = (index: number) => {
  todos.value.splice(index, 1);
  saveToLS(todos.value);
};

const handleToggle = (index: number) => {
  todos.value[index].done = !todos.value[index].done;
  saveToLS(todos.value);
};

const sortedTodos = computed(() => {
  return todos.value.map((todo) => ({ ...todo }));
});

const displayedTodos = computed(() => {
  if (displayCompletedTodos.value) {
    return sortedTodos.value.filter((todo) => todo.done);
  }
  return sortedTodos.value;
});

const completedTodos = computed(() => {
  return todos.value.filter((todo) => todo.done).length;
});

const totalTodos = computed(() => {
  return todos.value.length;
});

const clearTodos = () => {
  todos.value = [];
  saveToLS(todos.value);
};

function saveToLS(todos: Todo[]) {
  localStorage.setItem('todos', JSON.stringify(todos));
}
</script>

<template>
  <div class="todoWrapper">
    <AddTodo @addTodos="addTodo"></AddTodo>
    <div class="btns">
      <div v-if="displayedTodos.length > 0">
        <div v-if="displayCompletedTodos && completedTodos > 0">
          Completed todos: {{ completedTodos }} of {{ totalTodos }}
        </div>

        <button @click="displayCompletedTodos = false" class="todosBtn"
          >Todos {{ totalTodos }}</button
        >
        <button @click="displayCompletedTodos = true" class="completedBtn"
          >Done {{ completedTodos }}</button
        >
        <button @click="clearTodos" class="clearBtn">Clear</button>
      </div>
      <div v-else-if="displayCompletedTodos">
        No completed todos.

        <div v-if="displayCompletedTodos && completedTodos > 0">
          Completed todos: {{ completedTodos }} of {{ totalTodos }}
        </div>
        <button @click="displayCompletedTodos = false" class="todosBtn"
          >Todos {{ totalTodos }}</button
        ></div
      >
    </div>
    <div class="todoList">
      <ShowTodos
        v-for="(todo, index) in displayedTodos"
        :key="index"
        :todo="todo"
        :index="index"
        @toggle-todo="handleToggle(index)"
        @delete-todo="deleteTodo(index)"
        :displayCompletedTodos="displayCompletedTodos"
      ></ShowTodos>
    </div>
  </div>
</template>

<style scoped>
.todoWrapper {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  width: 350px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 5%;
  padding: 20px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  max-height: 450px;
}

.todoList {
  overflow-y: auto;
  overflow-x: auto;
  min-width: 200px;
  margin-top: 10px;
}
</style>
