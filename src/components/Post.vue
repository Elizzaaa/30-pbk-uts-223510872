<template>
  <div class="app">
    <header>
      <h1>Postingan Pengguna</h1>
      <select v-model="selectedUser" @change="fetchPosts">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
    </header>
    <main>
      <section v-if="loading" class="loading">Memuat...</section>
      <section v-else class="posts" v-if="posts.length">
        <div v-for="post in posts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';

const users = ref([]);
const selectedUser = ref('');
const posts = ref([]);
const loading = ref(false);

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    if (!response.ok) throw new Error('Failed to fetch users');
    const data = await response.json();
    users.value = data;
  } catch (error) {
    console.error(error);
  }
};

const fetchPosts = async () => {
  if (!selectedUser.value) return;
  loading.value = true; // Start loading
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
    if (!response.ok) throw new Error('Failed to fetch posts');
    const data = await response.json();
    posts.value = data;
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false; // End loading
  }
};

onMounted(() => {
  fetchUsers();
});

// Watch for changes in selectedUser to fetch posts automatically
watch(selectedUser, fetchPosts);
</script>

<style scoped>
/* Add some basic styles */
.app {
  font-family: Arial, sans-serif;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #f4f4f4;
}

.loading {
  text-align: center;
  padding: 2rem;
}

.posts {
  padding: 1rem;
}

.posts div {
  margin-bottom: 1.5rem;
}

.posts h3 {
  margin: 0 0 0.5rem;
}
</style>
