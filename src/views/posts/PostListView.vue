<template>
  <div>
    <h2>게시물목록</h2>
    <hr class="my-4" />
    <PostFilter v-model:title="params.title_like" v-model:limit="params._limit"></PostFilter>
    <hr class="my-4" />
    <div class="row g-3">
      <AppGrid :items="posts">
        <template v-slot="{ item }">
          <PostItem :title="item.title" :content="item.content" :created-at="item.createdAt" @click="goPage(item.id)"
            @modal="openModal(item)" @preview="selectPreview(item.id)"></PostItem>
        </template>
      </AppGrid>
    </div>
    <AppPagination :current-page="params._page" :page-count="pageCount" @page="page => params._page"></AppPagination>
    <AppModal :modelValue="show" :title="게시글" />
    <template v-if="posts && posts.lenth > 0">
      <hr class="my-5" />
      <AppCard>
        <PostDetailView :id="posts[0].id"></PostDetailView>
      </AppCard>
    </template>
  </div>
</template>

<script setup>
import PostItem from '@/components/PostItem.vue';
import PostDetailView from './PostDetailView.vue';
import AppCard from '@/components/AppCard.vue';
import { getPosts } from '@/api/posts';
import { ref, computed, watchEffect } from 'vue';
import { useRouter } from 'vue-router';
import AppPagination from '@/components/AppPagination.vue';
import AppGrid from '@/components/AppGrid.vue';
import PostFilter from '@/components/posts/PostFilter.vue';
import AppModal from '@/components/AppModal.vue';
const router = useRouter();
const posts = ref([]);
const params = ref({
  _sort: 'createdAt',
  _order: 'desc',
  _page: 1,
  _limit: 6,
  title_like: '',
});
const totalCount = ref(0);
const pageCount = computed(() => Math.ceil(totalCount.value / params.value._limit));

const fetchPosts = async () => {
  try {
    const { data, headers } = await getPosts(params.value);
    posts.value = data;
    totalCount.value = headers['x-total-count'];
  } catch (error) {
    console.error(error);
  }
};
fetchPosts();
const goPage = id => {
  // router.push(`/posts/${id}`);
  router.push({
    name: 'PostDetail',
    params: {
      id,
    },
  });
};
watchEffect(fetchPosts);

// modal
const show = ref(false);
const openModal = () => {
  show.value = true;
};
const closeModal = () => {
  show.value = false;
};
</script>

<style lang="scss" scoped>
</style>
