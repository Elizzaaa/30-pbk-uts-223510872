<template>
  <div class="container">
    <div class="todo-app">
      <h1>List Kegiatan!</h1>
      <p>Masukkan Kegiatan yang kalian sukai</p>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Masukkan Kegiatan" />
        <button @click="addTask">ADD</button>
      </div>
      <div class="filters">
        <button @click="hideCompleted = !hideCompleted" class="button">
          {{ hideCompleted ? 'Tampilkan' : 'Sembunyikan' }}
        </button>
      </div>
      <ul>
        <li
          v-for="(task, index) in filteredTodos"
          :key="task.id"
          :class="{ checked: task.checked }"
          @click="toggleTask(task)"
        >
          {{ task.text }}
          <span @click.stop="removeTask(task)">&#10005;</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const tasks = ref([])
const hideCompleted = ref(false)

const filteredTodos = computed(() => {
  return hideCompleted.value ? tasks.value.filter((task) => !task.checked) : tasks.value
})

function addTask() {
  if (newTask.value.trim() === '') {
    alert('Isi terlebih dahulu!')
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false })
    newTask.value = ''
    saveData()
  }
}

function toggleTask(task) {
  task.checked = !task.checked
  saveData()
}

function removeTask(task) {
  const index = tasks.value.findIndex((t) => t.id === task.id)
  if (index !== -1) {
    tasks.value.splice(index, 1)
    saveData()
  }
}

function saveData() {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

function loadData() {
  const savedTasks = localStorage.getItem('tasks')
  tasks.value = savedTasks ? JSON.parse(savedTasks) : []
}
loadData()
</script>

<style scoped>
.container {
  background: #efdad0;
  width: 100%;
  min-height: 50vh;
  padding: 20px;
  margin-top: 10px;
}

.todo-app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: #af998f;
}

.todo-app h1 {
  text-align: center;
  color: #ecd5d5;
  margin-bottom: 20px;
  font-style: italic;
  font-size: 24px;
}

.todo-app p {
  text-align: center;
  color: #ecd5d5;
  margin-bottom: 20px;
  font-size: 18px;
}

.row {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

input[type='text'] {
  flex: 1;
  padding: 8px;
  font-size: 16px;
}

button {
  background-color: #000;
  color: #fff;
  border: none;
  padding: 8px 16px;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background-color: #424242;
}

ul {
  padding: 0;
}

ul li {
  position: relative;
  font-size: 18px;
  padding: 10px 10px 10px 50px;
  cursor: pointer;
  list-style-type: none;
}

ul li.checked {
  text-decoration: line-through;
  color: #cfcfcf;
}

ul li span {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  font-size: 24px;
  cursor: pointer;
}

.filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.filters button {
  padding: 8px 16px;
  font-size: 16px;
}
</style>
