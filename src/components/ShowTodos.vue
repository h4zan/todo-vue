<script setup lang="ts">
import { ref, watchEffect } from 'vue';
import { Todo } from '../models/Todo';

interface IShowTodoProps {
  todo: Todo;
  index: number;
  displayCompletedTodos: boolean;
}

const props = defineProps<IShowTodoProps>();

const emit = defineEmits(['toggle-todo', 'delete-todo']);

const handleTodoToggle = () => {
  props.todo.done = !props.todo.done;
  emit('toggle-todo');
};
</script>

<template>
  <div class="todo" :style="{ backgroundColor: props.todo.backgroundColor }">
    <div class="todoTitle">
      <label class="checkbox-label">
        <input
          type="checkbox"
          v-model="props.todo.done"
          @change="handleTodoToggle"
          class="checkbox-input"
          aria-label="Toggle todo"
        />
        <span class="checkbox-custom"></span>
      </label>
      <span :class="{ done: props.todo.done }">{{ props.todo.text }}</span>
    </div>
    <button @click="$emit('delete-todo')" class="deleteBtn">X</button>
  </div>
</template>

<style scoped>
span.done {
  text-decoration: line-through;
}

.todo {
  width: 275px;
  word-break: break-all;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin: 3px;
  border-radius: 3px;
}

.todoTitle {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.checkbox-label {
  position: relative;
  padding-left: 25px;
  cursor: pointer;
  margin-left: 10px;
  padding-bottom: 10px;
}

.checkbox-input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.checkbox-custom {
  position: absolute;
  top: 0;
  left: 0;
  width: 10px;
  height: 10px;
  background-color: none;
  border: 0.5px black solid;
}

.checkbox-input:checked ~ .checkbox-custom {
  background-color: black;
}

.checkbox-input:checked ~ .checkbox-custom:after {
  display: block;
}

.checkbox-custom:after {
  left: 6px;
  top: 2px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}
</style>
