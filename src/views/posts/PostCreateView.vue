<template>
  <div>
    <h2>게시글 등록</h2>
    <hr class="my-4" />
    <PostForm v-model:title="form.title" v-model:content="form.content" @submit.prevent="save">
      <template #actions>
        <button type="button" class="btn btn-outline-dark" @click="goListPage">
          목록
        </button>
        <button class="btn btn-primary" :disabled="loading">
          <template v-if="loading">
            <span class="spinner-grow spinner-grow-sm" role="status" aria-hidden="true"></span>
            <span class="visually-hidden">Loading...</span>
          </template>
          <template v-else> 저장 </template>
        </button>
      </template>
    </PostForm>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { createPost } from '@/api/posts';
import PostForm from '@/components/posts/PostForm.vue';

const router = useRouter();
const form = ref({
  title: null,
  content: null,
});

const goListPage = () => router.push({ name: 'PostList' });

const save = async () => {
  try {
    const data = {
      ...form.value,
      createdAt: Date.now(),
    };
    await createPost(data);
    goListPage();
  } catch (error) {
    console.error(error);
  }
};
</script>

<style lang="scss" scoped>
</style>
