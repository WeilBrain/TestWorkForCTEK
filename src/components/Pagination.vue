<template>
  <div class="pagination__wrapper">
    <button class="pagination__btn back" @click="prevPage" :disabled="currentPage === 1">Назад</button>
    <span class="pagination__current">{{ currentPage }}</span>
    <button class="pagination__btn next" @click="nextPage" :disabled="currentPage === totalPages">Вперёд</button>
  </div>
</template>

<script setup>
import {computed, ref, watch} from 'vue';

const props = defineProps(['total', 'perPage']);
const emit = defineEmits(['page']);

const currentPage = ref(1);
const totalPages = computed(() => Math.ceil(props.total / props.perPage));

console.log(props.total)
console.log(props.perPage)

const prevPage = () => {
  if (currentPage.value >= 1) {
    currentPage.value--;
    emit('page', currentPage.value);
  }
};

const nextPage = () => {
  if (currentPage.value <= totalPages.value) {
    currentPage.value++;
    emit('page', currentPage.value);
  }
};

watch(currentPage, (newPage) => {
  emit('page', newPage);
  console.log(newPage)
});
</script>

<style lang="scss" scoped>
.pagination {
  &__current {
    font-size: 18px;
    padding: 20px;
  }
}
</style>
