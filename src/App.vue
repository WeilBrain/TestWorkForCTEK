<template>
  <main>
    <section class="guide">
      <h1 class="guide__title">Справочник</h1>
      <div class="guide__search-wrapper">
        <div class="guide__nav">
          <input class="guide__search--input" v-model="search" placeholder="Найти по ФИО"/>
          <button @click="showModal = true" class="guide__button-add">Добавить</button>
        </div>

        <Modal class="guide__modal" v-if="showModal" @close="showModal = false" @add="addOrganization" />

        <OrganizationTable
            class="guide__organization"
            :organizations="paginatedOrganizations"
            @sort="setSortKey"
            @remove="removeOrganization"
        />

        <Pagination
            class="guide__pagination"
            :total="filteredOrganizations.length"
            :perPage="itemsPerPage"
            @page="handlePageChange"
        />
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, computed } from 'vue';
import OrganizationTable from './components/OrganizationTable.vue';
import Pagination from './components/Pagination.vue';
import Modal from './components/Modal.vue';

const organizations = ref([
  { name: 'Организация А', director: 'Иванов Иван Иванович', phone: '1234567890' },
  { name: 'Организация Б', director: 'Петров Петр Петрович', phone: '0987654321' },
  { name: 'Организация В', director: 'Сидоров Сидор Сидорович', phone: '1122334455' },
  { name: 'Организация Г', director: 'Кузнецов Александр Сергеевич', phone: '2233445566' },
  { name: 'Организация Д', director: 'Новиков Николай Николаевич', phone: '3344556677' },
  { name: 'Организация Е', director: 'Федоров Федор Федорович', phone: '4455667788' },
  { name: 'Организация Ж', director: 'Григорьев Григорий Григорьевич', phone: '5566778899' },
  { name: 'Организация З', director: 'Михайлов Михаил Михайлович', phone: '6677889900' },
  { name: 'Организация И', director: 'Васильев Василий Васильевич', phone: '7788990011' },
  { name: 'Организация К', director: 'Егоров Егор Егорович', phone: '8899001122' },
]);

const search = ref('');
const showModal = ref(false);
const currentPage = ref(1);
const itemsPerPage = 5;
const sortKey = ref('');
const sortOrder = ref(1);

const filteredOrganizations = computed(() => {
  return organizations.value.filter(org =>
      org.director.toLowerCase().includes(search.value.toLowerCase())
  );
});

const paginatedOrganizations = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  const end = start + itemsPerPage;
  return filteredOrganizations.value.slice(start, end);
});

const addOrganization = (org) => {
  organizations.value.push(org);
};

const removeOrganization = (index) => {
  const globalIndex = (currentPage.value - 1) * itemsPerPage + index;
  organizations.value.splice(globalIndex, 1);
};

const setSortKey = ({ key, order }) => {
  sortKey.value = key;
  sortOrder.value = order;
  organizations.value.sort((a, b) => {
    const result = a[key].localeCompare(b[key]);
    return result * order;
  });
};

const handlePageChange = (page) => {
  currentPage.value = page;
};
</script>

<style scoped lang="scss">
.guide {
  &__title {
    text-align: center;
  }

  &__search-wrapper {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    margin-bottom: 20px;
  }

  &__search--input {
    padding: 10px;
    margin-right: 10px;
  }

  &__pagination{
    margin-top: 20px;
  }

  &__nav{
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
}
</style>
