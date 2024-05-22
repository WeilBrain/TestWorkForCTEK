<template>
  <div class="table">
    <div class="table-header">
      <div class="table-cell header" @click="sort('name')">
        Название <span v-if="sortKey === 'name'">{{ sortOrder === 1 ? '▲' : '▼' }}</span>
      </div>
      <div class="table-cell header" @click="sort('director')">
        ФИО директора <span v-if="sortKey === 'director'">{{ sortOrder === 1 ? '▲' : '▼' }}</span>
      </div>
      <div class="table-cell header">Телефон</div>
      <div class="table-cell header">Удалить</div>
    </div>
    <div class="table-body">
      <div class="table-row" v-for="(org, index) in organizations" :key="index">
        <div class="table-cell">{{ org.name }}</div>
        <div class="table-cell">{{ org.director }}</div>
        <div class="table-cell">{{ org.phone }}</div>
        <div class="table-cell">
          <button class="remove-btn" @click="$emit('remove', index)">❌</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps(['organizations']);
const emit = defineEmits(['sort', 'remove']);

const sortKey = ref('');
const sortOrder = ref(1);

const sort = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = -sortOrder.value;
  } else {
    sortKey.value = key;
    sortOrder.value = 1;
  }
  emit('sort', { key, order: sortOrder.value });
};
</script>

<style scoped>
.table {
  display: flex;
  flex-direction: column;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
}

.table-header, .table-row {
  display: flex;
  border-bottom: 1px solid #ddd;
}

.table-cell {
  flex: 1;
  padding: 10px;
  text-align: left;
}

.header {
  font-weight: bold;
  transition: .3s ease-in-out;
  &:nth-child(-n+2):hover {
    cursor: pointer;
    background-color: gray;
    transition: .3s ease-in-out;
  }
}

.remove-btn:hover {
  color: #ffcccc;
}
</style>
