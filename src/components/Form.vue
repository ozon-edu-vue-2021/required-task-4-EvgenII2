<template>
  <div class="form">
    <h1>Vue-форма</h1>
    <h2>Личные данные</h2>
    <div class="components-wrapper">
      <div>
        <h3>Фамилия: {{ lastname }}</h3>
        <base-input
          v-model="lastname"
          placeholder="Введите фамилию"
          autofocus
        />
      </div>
      <div>
        <h3>Имя: {{ firstname }}</h3>
        <base-input v-model="firstname" placeholder="Введите имя" autofocus />
      </div>
      <div>
        <h3>Отчество: {{ patronymic }}</h3>
        <base-input
          v-model="patronymic"
          placeholder="Введите отчество"
          autofocus
        />
      </div>
      <div>
        <h3>Дата рождения: {{ birthday }}</h3>
        <base-input v-model="birthday" placeholder="Введите дату" autofocus />
      </div>
      <div>
        <h3>E-mail: {{ email }}</h3>
        <base-input v-model="email" placeholder="Введите e-mail" autofocus />
      </div>

      <!-- <div>
        <h2>NumberInput: {{ numtext }}</h2>
        <number-input
          v-model="numtext"
          placeholder="Введите число"
          :max="501"
        />
      </div>-->
      <div>
        <h2>Nationality</h2>
        <select v-model="nationality">
          <option
            v-for="(country, id) in countries"
            :key="id"
            v-bind:value="country.nationality"
          >
            {{ country.nationality }}
          </option>
        </select>
        <!-- <select :options="countries" label="nationality"></select> -->
      </div>
    </div>
  </div>
</template>

<script>
import BaseInput from "./Input/BaseInput.vue";
// import NumberInput from "./Input/NumberInput.vue";
// import MultiSelect from "./MultiSelect/MultiSelect.vue";
import countries from "../assets/data/citizenships.json";
console.log(countries);
export default {
  components: {
    BaseInput,
    // NumberInput,
    // MultiSelect,
  },
  data() {
    return {
      firstname: "",
      lastname: "",
      patronymic: "",
      birthday: "",
      email: "",
      isMan: true,
      nationality: "",
      rusPasportSeries: null,
      rusPasportNumber: null,
      rusPasportDate: null,
      hasChangedFirstnameOrLastname: false,
      oldFirstname: null,
      oldLastname: null,
      foreignFirstname: null,
      foreignLastname: null,
      foreignPasportType: null,
      foreignPasportNumber: null,
      foreignPasportCountry: null,

      countries: countries,
      // numtext: "",

      selectedItems: [countries[0].nationality, countries[3].nationality],
    };
  },
  methods: {
    updateSelectedItems(item) {
      const foundIndex = this.selectedItems.findIndex(
        (selected) => selected.id === item.id
      );
      foundIndex > -1
        ? this.removeFromSelected(item)
        : this.addToSelected(item);
    },
  },
};
</script>

<style>
.form {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<style scoped>
.components-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 60px;
  margin: 0 auto;
  width: 1000px;
}

.component-item {
  display: flex;
  margin-top: 30px;
}

.component-chip-item:not(:last-child) {
  margin-right: 8px;
}
</style>
