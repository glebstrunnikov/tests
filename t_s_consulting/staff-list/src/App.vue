<template>
  <div class="d-flex flex-column min-vh-100">
    <!-- Компонент хедера, простой и без логики, находится в папке overlay -->
    <the-header></the-header>
    <!-- Компонент таблицы, в котором отображаются данные. Каждая строка - дочерний компонент. И то, и другое - в папке table -->
    <data-table
      @deleteLine="deletePerson"
      @editLine="editPerson"
      :staff="data"
    ></data-table>
    <!-- Кнопка для добавления записи, настолько проста, что не стал даже в отдельный компонент выделять -->
    <div @click="addPerson" class="btn btn-success" style="align-self: center">
      <h3>Add Person</h3>
    </div>
    <!-- Простой компонент футера, в папке layout -->
    <the-footer class="mt-auto"></the-footer>
    <!-- Компонент модальнка, при вызове перекрывает весь экран полупрозрачным оверлеем, содержит слот для вывода собственно модального окна с интерфейсом редактирования записи или предупреждением о ее удалении -->
    <the-modal @hideOverlay="modalClose" v-if="modalActive">
      <delete-warning v-if="modalHead === 'Delete Person'"></delete-warning>
      <data-form
        v-if="modalHead === 'Add Person' || modalHead === 'Edit Person'"
      ></data-form>
    </the-modal>
  </div>
</template>

<script setup>
import TheHeader from "./components/layout/TheHeader.vue";
import TheFooter from "./components/layout/TheFooter.vue";
import DataTable from "./components/table/DataTable.vue";
import TheModal from "./components/modal/TheModal.vue";
import DataForm from "./components/modal/DataForm.vue";
import DeleteWarning from "./components/modal/DeleteWarning.vue";
import { ref, provide } from "vue";

// Модальное окно используется для редактирования данных сотрудника или ввода нового, а также для подтверждения удаления. По умолчанию оно скрыто, показывается, если modalActive=true
const modalActive = ref(false);
// Импортируем данные: если они есть по нужному ключу в localStorage, то они и используется, если их там нет, то подгружается dummy
import dummyData from "./dummy_data";
const importedData = localStorage.getItem("staff")
  ? JSON.parse(localStorage.getItem("staff"))
  : dummyData;
const data = ref(importedData);
provide("staff", data);
// modalHead - переменная, которая определяет, что будет в заголовке у модального окна. Также она используется в компонентах внутри модальника, чтобы определить тип окна
const modalHead = ref();
provide("modalHead", modalHead);
// при каждом открытии модальника данные сохраняются в localStorage. Это делается для того, чтобы если пользователь их модифицировал, но вышел без сохранения, откатить изменения
function modalOpen() {
  modalActive.value = true;
  localStorage.setItem("staff", JSON.stringify(data.value));
}
// при закрытии модальника данные, наоборот, берутся из localStorage
function modalClose() {
  modalActive.value = false;
  data.value = JSON.parse(localStorage.getItem("staff"));
}
// функция saveData используется, чтобы сохранить данные в localStorage при их сохранении пользователем. Потом вызывается modalClose
function saveData() {
  localStorage.setItem("staff", JSON.stringify(data.value));
  modalClose();
}
provide("modalOpen", modalOpen);
provide("modalClose", modalClose);
provide("saveData", saveData);

// personOfInterest - это id записи из таблицы, которую требуется удалить/редактировать. Это число появляется, когда соответствующий компонент (строка, BasicLine.vue) передает значение через emit в компонент таблицы DataTable.vue, а тот таким же образом - сюда. Далее, имея этот id, мы модифицируем данные data, а они автоматически меняются во всех остальных компонентах, так как передаются отсюда в виде ref через provide
const personOfInterest = ref();
provide("personOfInterest", personOfInterest);
// editPerson и deletePerson отличаются только заголовком модальника. Но исходя из этого заголовка отображается тот или иной тип модальника: редактирование данных или предупреждение об удалении
function editPerson(id) {
  modalHead.value = "Edit Person";
  personOfInterest.value = id;
  modalOpen();
}
function deletePerson(id) {
  personOfInterest.value = id;
  modalHead.value = "Delete Person";
  modalOpen();
}
// при открытии предупреждения об удалении есть две кнопки. При нажатии на одну из них мы вырезаем из массива данных нужный объект и сохраняем этот массив в localStorage. Это работает по функции confirmDelete, которую мы передаем в DeleteWarning через provide. Если же пользователь просто закрывает модальник, то ничего не происходит.
function confirmDelete(id) {
  data.value.splice(id, 1);
  localStorage.setItem("staff", JSON.stringify(data.value));
  modalClose();
}
provide("confirmDelete", confirmDelete);

// добавление записи в целом работает так же, как и ее редактирование, только мы вначале добавляем к массиву данных пустой объект, а потом принимаем id (personOfInterest) в виде последнего объекта в нем
function addPerson() {
  modalHead.value = "Add Person";
  modalOpen();
  data.value.push({
    firstName: "",
    lastName: "",
    experience: 0,
    age: 0,
    address: "",
  });
  personOfInterest.value = data.value.length - 1;
}
</script>
