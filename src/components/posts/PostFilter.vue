<template>
  <form @submit.prevent>
    <div class="row g-3">
      <div class="col">
        <input :value="title" @input="$emit('update:title', $event.target.value)" type="text" class="form-controle"
          placeholder="검색어 입력" />
      </div>
      <div class="col-3">
        <select :value="limit" @input="$emit('update:limit', $event.target.value)" class="form-select">
          <option value="3">3개</option>
          <option value="6">6개</option>
          <option value="9">9개</option>
        </select>
      </div>
    </div>
  </form>
</template>

<script setup>
defineProps({
  title: String,
  limit: Number,
});
defineEmits(['update:title', 'update:limit']);
</script>

<style lang="scss" scoped>
</style>