<template>
  <!-- компонент получает данные из одного объекта общего массива. Он же передает при помощи emit id строки при нажатии на карандаш/крест -->
  <tr>
    <th scope="row">{{ props.id }}</th>
    <td>{{ props.firstName }}</td>
    <td>{{ props.lastName }}</td>
    <td>{{ props.experience }} years</td>
    <td>{{ props.age }} years</td>
    <td>{{ props.address }}</td>
    <td>
      <span class="hover" @click="editLine(props.id)">✏️</span>
      <span class="hover" @click="deleteLine(props.id)">❌</span>
    </td>
  </tr>
</template>

<script setup>
const emit = defineEmits(["deleteLine", "editLine"]);
function deleteLine(id) {
  emit("deleteLine", id);
}
function editLine(id) {
  emit("editLine", id);
}

const props = defineProps([
  "id",
  "firstName",
  "lastName",
  "experience",
  "age",
  "address",
]);
</script>

<style scoped>
.hover:hover {
  cursor: pointer;
}
</style>
