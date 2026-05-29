<script setup>
import axios from 'axios';
import { reactive } from 'vue';
import router from '@/router';

const form = reactive({
  title: '',
  description: ''
})

const handleSubmit = async () => {
  const newPost = {
    title: form.title,
    description: form.description
  }
  try {
    const response = await axios.post('http://localhost:8080/posts', newPost);
    return router.push('/posts');
  } catch (error) {
    console.error("Error on posting data:", error);
  }
}
</script>

<template>
  <div class="center-container">
    <RouterLink :to="`/posts`" class="btn btn-green">Return to Posts</RouterLink>
    <div>
      <h1>Add New Post</h1>
      <div class="form-group">
        <label>Title</label>
        <input v-model="form.title" type="text" placeholder="Post title" />
      </div>
      <div class="form-group">
        <label>Description</label>
        <textarea v-model="form.description" rows="4" placeholder="Post description"></textarea>
      </div>
      <button class="btn btn-green" @click="handleSubmit">Submit</button>
    </div>
  </div>
</template>
