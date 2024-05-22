<template>
  <div class="modal">
    <div class="modal-content">
      <h2 class="modal__title">Добавить организацию</h2>
      <div class="modal__wrapper">
        <label class="modal__label">
          Название:
          <input class="modal__input" v-model="name" type="text" />
        </label>
        <label class="modal__label">
          ФИО директора:
          <input class="modal__input" v-model="director" type="text" />
        </label>
        <label class="modal__label">
          Телефон:
          <input class="modal__input" v-model="phone" type="text" />
        </label>
        <div class="modal__wrapper-btn">
          <button class="modal-btn add" @click="add" :disabled="!isValid">ОК</button>
          <button class="modal-btn close" @click="close">Отмена</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const emit = defineEmits(['close', 'add']);
const name = ref('');
const director = ref('');
const phone = ref('');

const isValid = computed(() => {
  return name.value && director.value && phone.value;
});

const add = () => {
  if (isValid.value) {
    emit('add', { name: name.value, director: director.value, phone: phone.value });
    close();
  }
};

const close = () => {
  name.value = '';
  director.value = '';
  phone.value = '';
  emit('close');
};
</script>

<style lang="scss" scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  background: #2c2c2c;
  padding: 20px;
  border-radius: 8px;
}

.modal {
  &__wrapper {
    display: grid;
    grid-template-columns: 1fr;
  }

  &__label {
    display: flex;
    flex-direction: column;
    &:not(:last-child){
      margin-bottom: 10px;
    }
  }

  &__input{
    margin-top: 5px;
    padding: 10px;
  }

  &-btn{
    margin: 5px;
  }
}

</style>
