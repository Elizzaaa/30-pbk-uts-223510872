<template>
  <div class="container">
    <div class="todo-app">
      <h1>List Kegiatan!</h1>
      <h2>Masukkan Kegiatan yang kalian sukai</h2>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Masukkan kegiatan">
        <button @click="addTask">ADD</button>
      </div>
      <div class="filters">
        <button @click="hideCompleted = !hideCompleted" class="button-74">
          {{ hideCompleted ? 'Tampilkan' : 'Sembunyikan' }}
        </button>
      </div>
      <ul>
        <li v-for="(task, index) in filteredTodos" :key="task.id" :class="{ checked: task.checked }" @click="toggleTask(task)">
          {{ task.text }}
          <span @click.stop="removeTask(task)">&#xd7;</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter(task => !task.checked)
    : tasks.value;
});

function addTask() {
  if (newTask.value.trim() === '') {
    alert("Isi terlebih dahulu!");
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false });
    newTask.value = '';
    saveData();
  }
}

function toggleTask(task) {
  task.checked = !task.checked;
  saveData();
}

function removeTask(task) {
  const index = tasks.value.findIndex(t => t.id === task.id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
    saveData();
  }
}

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function loadData() {
  const savedTasks = localStorage.getItem("tasks");
  tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
}

loadData();
</script>

<style scoped>
.container {
  background: rgb(239, 214, 176);
  width: 100%;
  min-height: 50vh;
  padding: 20px;
  margin: 10px auto;
}

.todo-app {
  width: 100%;
  max-width: 600px;
  background: rgb(175, 153, 143);
  margin: 0 auto;
  padding: 20px;
}

.todo-app h1, .todo-app h2 {
  text-align: center;
  color: rgb(236, 213, 213);
  margin-bottom: 30px;
  font-style: oblique;
}

input {
  flex: 1;
  border: 1px solid #ccc;
  background: #dbdbc7;
  padding: 8px;
  font-size: 16px;
  color: #000;
}

.row button {
  outline: none;
  padding: 10px;
  background: rgb(0, 0, 0);
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.row button:hover {
  background: #333;
}

ul {
  padding: 0;
  list-style: none;
}

ul li {
  font-size: 18px;
  padding: 10px;
  cursor: pointer;
  position: relative;
  background: #fff;
  margin-bottom: 5px;
}

ul li.checked {
  text-decoration: line-through;
  color: #cfcfcf;
}

ul li span {
  position: absolute;
  right: 10px;
  top: 10px;
  cursor: pointer;
  font-size: 18px;
  color: #e03a3a;
}

ul li span:hover {
  background: blue;
}

.filters {
  display: flex;
  justify-content: center;
  padding-top: 20px;
}

.button-74 {
  background-color: #000;
  color: rgb(238, 188, 137);
  cursor: pointer;
  padding: 10px 20px;
  font-size: 15px;
  text-align: center;
}

.button-74:hover {
  background-color: #424242;
}
</style>
