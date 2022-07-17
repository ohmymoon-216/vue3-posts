<template>
  <div>
    <header>
      <nav class="navbar navbar-expand-sm navbar-dark bg-primary">
        <a class="navbar-brand" href="#">GYM CODING</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto">
            <li class="nav-item">
              <RouterLink class="nav-link" actove-class="active" to="/">Home</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" actove-class="active" to="/about">About</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" actove-class="active" to="/posts">게시글</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" actove-class="active" to="/nested">Nested</RouterLink>
            </li>
          </ul>
          <div class="d-flex">
            <button class="btn btn-outline-light my-2 my-sm-0" type="button" @click="goPage">
              글쓰기
            </button>
          </div>
        </div>
      </nav>
    </header>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
const router = useRouter();
const goPage = () => {
  router.push({
    name: 'PostCreate',
  });
};
</script>

<style lang="scss" scoped>
</style>
