<script setup>
import { ref } from 'vue';
import Todo from './components/Todo.vue';
import Post from './components/Post.vue';

const selectedMenu = ref('');

const handleMenuClick = (menu) => {
  selectedMenu.value = menu;
};
</script>

<template>
  <div>
    <button @click="handleMenuClick('Todos')">Todos</button>
    <button @click="handleMenuClick('Post')">Post</button>

    <div>
      <!-- Display Todo component if 'Todos' is selected -->
      <Todo v-if="selectedMenu === 'Todos'" />

      <!-- Display Post component if 'Post' is selected -->
      <Post v-if="selectedMenu === 'Post'" />

      <!-- Message when no menu is selected -->
      <p v-if="!selectedMenu">No menu selected</p>
    </div>
  </div>
</template>
