<script setup>
import axios from 'axios';
import { onMounted, reactive } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const router = useRouter();

const postId = route.params.id;

const form = reactive({
  title: '',
  description: ''
})

onMounted(async () => {
  try {
    const response = await axios.get(`http://localhost:8080/posts/${postId}`);
    form.title = response.data.title;
    form.description = response.data.description;
  } catch (error) {
    console.log("Error fetching post", error);
  }
})

const handleSubmit = async () => {
  const updatePost = {
    title: form.title,
    description: form.description
  }
  try {
    await axios.put(`http://localhost:8080/posts/${postId}`, updatePost);
    router.push('/posts');
  } catch (error) {
    console.error("Error updating post:", error);
  }
}
</script>

<template>
  <div class="center-container">
    <RouterLink :to="`/posts`" class="btn btn-green">Return to Posts</RouterLink>
    <div>
      <h1>Edit Post</h1>
      <div class="form-group">
        <label>Title</label>
        <input v-model="form.title" type="text" />
      </div>
      <div class="form-group">
        <label>Description</label>
        <textarea v-model="form.description" rows="4"></textarea>
      </div>
      <button class="btn btn-blue" @click="handleSubmit">Update</button>
    </div>
  </div>
</template>
