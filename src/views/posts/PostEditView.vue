<template>
  <div>
    <h2>게시물수정</h2>
    <hr class="my-4" />
    <PostForm v-model:title="form.title" v-model:content="form.content" @submit.prevent="edit">
      <template #actions>
        <button type="button" class="btn btn-outline-danger" @click="goDetailPage">
          취소
        </button>
        <button class="btn btn-primary" :disabled="editLoading">
          <template v-if="editLoading">
            <span class="spinner-grow spinner-grow-sm" role="status" aria-hidden="true"></span>
            <span class="visually-hidden">Loading...</span>
          </template>
          <template v-else> 수정 </template>
        </button>
      </template>
    </PostForm>
    <AppAlert :items="items" :message="alertMessage" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { getPostById, updatePost } from '@/api/posts';
import PostForm from '@/components/posts/PostForm.vue';
import AppAlert from '@/components/AppAlert.vue';

const route = useRoute();
const router = useRouter();
const id = route.params.id;

const form = ref({
  title: null,
  content: null
});

const fetchPost = async () => {
  try {
    const { data } = await getPostById(id);
    setForm(data);
  } catch (error) {
    console.error(error);
    vAlert(error.message);
  }
}
fetchPost();

const goListPage = () => router.push({ name: 'PostList' });
const setForm = ({ title, content }) => {
  form.value.title = title;
  form.value.content = content;
};
const edit = async () => {
  try {
    await updatePost(id, { ...form.value });
    vAlert('수정이 완료되었습니다.', 'success');
    router.push({ name: 'PostDetail', params: { id } });
  } catch (error) {
    console.error(error);
    vAlert(error.message);
  }
};

// alert
const alerts = ref([]);

const vAlert = (message, type = 'error') => {
  alerts.value.push({ message, type });
  setTimeout(() => {
    alerts.value.shift();
  }, 2000);
};
</script>

<style scoped>
</style>
