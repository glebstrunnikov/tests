<template>
  <div class="m-5 table-responsive">
    <table class="table align-middle table-bordered">
      <!-- Статичный заголовок таблицы -->
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">First Name</th>
          <th scope="col">Last Name</th>
          <th scope="col">Experience</th>
          <th scope="col">Age</th>
          <th scope="col">Address</th>
          <th scope="col">Edit/Delete</th>
        </tr>
      </thead>
      <tbody>
        <!-- Строки таблицы. Id - номер строки, он же номер объекта в массиве данных -->
        <BaseLine
          v-for="(el, index) in staff"
          :key="index"
          :id="index"
          :firstName="el.firstName"
          :lastName="el.lastName"
          :experience="el.experience"
          :age="el.age"
          :address="el.address"
          @deleteLine="deleteLine"
          @editLine="editLine"
        ></BaseLine>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import BaseLine from "./BaseLine.vue";
import { inject } from "vue";
// Данные принимаются сюда через inject, а потом таблица строится через v-for с использованием props-ов. Также этот компонент принимает от дочерних и передает в App.vue id объекта в массиве данных, который надо удалить/редактировать.
const staff = inject("staff");
const emit = defineEmits(["deleteLine", "editLine"]);
function deleteLine(id) {
  emit("deleteLine", id);
}
function editLine(id) {
  emit("editLine", id);
}
</script>
