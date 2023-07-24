<script setup lang="ts">
import { ref } from 'vue';

const userInput = ref('');
const showWarning = ref(false);

const emit = defineEmits(['addTodos']);

const handleSubmit = () => {
  if (userInput.value.trim() === '') {
    showWarning.value = true;
    return;
  }

  emit('addTodos', userInput.value);
  userInput.value = '';
  showWarning.value = false;
};
</script>

<template>
  <div class="introWrapper">
    <div class="intro">
      <div class="iconContainer">
        <img
          class="titleIcon"
          src="https://i.postimg.cc/fyC6s7MV/note.png"
          alt="Clipboard, pixel art"
        />
      </div>
      <h3>Todo List</h3>
    </div>
    <div class="TodoEnter">
      <form @submit.prevent="handleSubmit">
        <input
          v-model="userInput"
          placeholder="Add a new todo"
          class="todoInput"
          maxlength="20"
          aria-label="Add a new todo"
        />
        <button type="submit" class="addBtn">+</button>
      </form>
      <div v-if="showWarning" class="warning">Please enter a valid todo.</div>
    </div>
  </div>
</template>

<style scoped>
.introWrapper {
  padding: 10px;
  border-radius: 3px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.intro {
  display: flex;
  align-items: center;
  gap: 10px;
}

.titleIcon {
  height: 20px;
  width: 20px;
}

.todoInput {
  border: 2px white ridge;
  width: 250px;
  background-color: white;
  box-shadow: 0.5px 0.5px 0.5px black;
}

form {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  padding: 10px;
}

.warning {
  color: #db4437;
  margin-top: 5px;
  text-align: center;
}
</style>
