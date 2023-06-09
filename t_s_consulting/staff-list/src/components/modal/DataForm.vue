<template>
  <div class="card-header pt-3">
    <!-- Шапка модальника, напоминает пользователю, что он тут делает -->
    <h4>{{ modalHead }}</h4>
  </div>
  <div class="card-body">
    <!-- Форма для редактирования данных -->
    <form @submit.prevent="saveData">
      <input
        v-model="personOfInterest.firstName"
        type="text"
        class="form-control mb-2"
        placeholder="First Name"
      />
      <input
        v-model="personOfInterest.lastName"
        type="text"
        class="form-control mb-2"
        placeholder="Last Name"
      />
      <div class="input-group">
        <input
          v-model="personOfInterest.experience"
          type="number"
          class="form-control mb-2"
          placeholder="Experience"
        />
        <span class="input-group-text mb-2">experience, years</span>
      </div>
      <div class="input-group">
        <input
          v-model="personOfInterest.age"
          type="number"
          class="form-control mb-2"
          placeholder="Age"
        />
        <span class="input-group-text mb-2">age, years</span>
      </div>

      <input
        v-model="personOfInterest.address"
        type="text"
        class="form-control mb-2"
        placeholder="Address"
      />
      <div class="float-end">
        <!-- Кнопка Save сохраняет введенные данные в соответствующем объекте массива -->
        <button
          :disabled="!formIsValid"
          type="submit"
          class="btn btn-success my-2"
        >
          <h5>Save</h5>
        </button>
        <!-- Кнопка Cancel закрывает модальник без сохранения. Точно так же, как и клик вне формы -->
        <div @click="modalClose" class="btn btn-info ms-3 my-2">
          <h5>Cancel</h5>
        </div>
      </div>
    </form>
    <div class="p-2" v-if="!formIsValid">
      Name, age and address must be present
    </div>
  </div>
</template>

<script setup>
import { inject, reactive, ref, watch } from "vue";
const modalClose = inject("modalClose");
const staff = inject("staff");
const id = inject("personOfInterest");
const saveData = inject("saveData");
const modalHead = inject("modalHead");
// personOfInterest в данном случае - не id, как в App.vue, а реактивный объект, значение которого зависит от данных и переданного сюда id
const personOfInterest = reactive(staff.value[id.value]);

// Вся логика, относящаяся к данным, вынесена в компонент App.vue. Этот компонент только получает данные из массива и вызывает функции, полученные через inject. Единственное исключение - данные, которые относятся непосредственно к форме. Все просто: при каждом изменении любого свойства personOfInterest запускается вотчер, который проверяет его свойства простейшим валидатором (есть имя, фамилия и адрес, возраст не равен нулю) и если результат неудовлетворителен, блокирует кнопку Save. Здесь же прописан ограничитель, не позволяющий указать отрицательный возраст или опыт работы
const formIsValid = ref(modalHead.value === "Add Person" ? false : true);
watch(personOfInterest, () => {
  if (
    !personOfInterest.firstName ||
    !personOfInterest.lastName ||
    !personOfInterest.age ||
    !personOfInterest.address
  ) {
    formIsValid.value = false;
  } else {
    formIsValid.value = true;
  }
  console.log(personOfInterest.age < 0);
  personOfInterest.age < 0 ? (personOfInterest.age = 0) : personOfInterest.age;
  personOfInterest.experience < 0
    ? (personOfInterest.experience = 0)
    : personOfInterest.experience;
});
</script>
