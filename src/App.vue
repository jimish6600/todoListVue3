<script setup>
import { ref } from "vue";

// Reactive state
const newTask = ref("");
const tasks = ref([]);
const pendingTasks = ref(0);

// Add a new task
const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, completed: false });
    pendingTasks.value++;
    newTask.value = "";
  }
};

// Remove a task
const removeTask = (index) => {
  tasks.value.splice(index, 1);
  pendingTasks.value--;
};

// Remove all tasks
const clearAll = () => {
  tasks.value = [];
  pendingTasks.value = 0;
}

// Remove all completed task
const removeCompleted = () => {
  tasks.value = tasks.value.filter(task=> !task.completed);
}

const checkCount = () => {
  pendingTasks.value = tasks.value.filter(task => !task.completed).length;
}
</script>

<template>
  <div class="main">
    <div class="app">
      <h1>To-Do List</h1>
      <div class="input-section">
        <input
          v-model="newTask"
          type="text"
          placeholder="Enter a new task"
          @keyup.enter="addTask"
        />
        <button @click="addTask">Add Task</button>
      </div>
      <ul class="todo-list">
        <li v-for="(task, index) in tasks" :key="index" class="todo-item">
          <label>
            <input type="checkbox" v-model="task.completed" @change="checkCount"/>
            <span :class="{ completed: task.completed }">{{ task.text }}</span>
          </label>
          <button @click="removeTask(index)">❌</button>
        </li>
      </ul>
      <div class="buttons">
        <button @click="clearAll">Clear All</button>
        <button @click="removeCompleted">Clear Completed</button>
      </div>
      <div>
        Pending Tasks : {{pendingTasks}}
      </div>
    </div>
  </div>
</template>

<style scoped>

.main {
  max-width: 500px;
  margin: auto;
  background: #00bec8;
  padding: 20px;
  border-radius: 12px;  
}

.app {
  font-family: Arial, sans-serif;
  text-align: center;
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 20px;
  background: #f9f9f9;
}
h1 {
  margin-bottom: 20px;
}
.input-section {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
input[type="text"] {
  flex: 1;
  padding: 8px;
  margin-right: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  padding: 8px 12px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
.todo-list {
  list-style-type: none;
  padding: 0;
}
.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
.todo-item button {
  background-color: #fb746b;
}
.todo-item button:hover {
  background-color: #e41e25;
}

.buttons {
  display: flex;
  justify-content: space-between;
  align-items: center;
  pedding: 10px;
}
</style>
