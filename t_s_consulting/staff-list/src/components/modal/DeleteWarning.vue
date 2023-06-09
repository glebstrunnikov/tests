<template>
  <div class="card-header pt-3">
    <!-- Шапка модальника, напоминает пользователю, что он тут делает -->
    <h4>{{ modalHead }}</h4>
  </div>
  <div class="card-body">
    <!-- Текст с именем и фамилией выбранного сотрудника, чтобы пользователь понимал, кого удаляет -->
    <h6>
      Are you sure you want to delete {{ personOfInterest.firstName }}
      {{ personOfInterest.lastName }}?
    </h6>
    <div class="float-end">
      <!-- Кнопка Delete вызывает функцию из App.vue, которая удаляет из массива выбранный объект -->
      <div @click="confirmDelete(id)" class="btn btn-success my-2">
        <h5>Delete</h5>
      </div>
      <!-- Кнопка Close закрывает модальник, не трогая массив данных -->
      <div @click="modalClose" class="btn btn-info ms-3 my-2">
        <h5>Cancel</h5>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject, reactive } from "vue";
const modalClose = inject("modalClose");
const modalHead = inject("modalHead");
const staff = inject("staff");
const id = inject("personOfInterest");
const confirmDelete = inject("confirmDelete");
const personOfInterest = reactive(staff.value[id.value]);
// здесь нет почти ничего интересного, компонент собран как упрощенная версия DataForm.vue. Принимает через inject те же данные, но обладает другим template-ом, вызывает функцию confirmDelete
</script>
