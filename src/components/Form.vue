<template>
  <div class="form" autocomplete="off">
    <h1>Vue-форма</h1>
    <h2>Личные данные</h2>
    <div class="components-wrapper">
      <div>
        <h3>Фамилия: {{ formData.lastname }}</h3>
        <input
          v-model="formData.lastname"
          placeholder="Введите фамилию"
          autofocus
        />
      </div>
      <div>
        <h3>Имя: {{ formData.firstname }}</h3>
        <input
          v-model="formData.firstname"
          placeholder="Введите имя"
          autofocus
        />
      </div>
      <div>
        <h3>Отчество: {{ formData.patronymic }}</h3>
        <input
          v-model="formData.patronymic"
          placeholder="Введите отчество"
          autofocus
        />
      </div>
      <div>
        <h3>Дата рождения: {{ formData.birthday }}</h3>
        <input
          v-model="formData.birthday"
          placeholder="Введите дату"
          autofocus
        />
      </div>
      <div>
        <h3>E-mail: {{ formData.email }}</h3>
        <input
          v-model="formData.email"
          placeholder="Введите e-mail"
          autofocus
        />
      </div>
      <div>
        <h3>Nationality</h3>
        <select v-model="formData.nationality">
          <option
            v-for="(country, id) in formData.countries"
            :key="id"
            v-bind:value="country.nationality"
          >
            {{ country.nationality }}
          </option>
        </select>
      </div>
      <div v-if="formData.nationality === 'Russia'">
        <h3>Серия паспорта: {{ formData.email }}</h3>
        <input
          v-model="formData.rusPasportSeries"
          placeholder="Введите серию паспорта"
          autofocus
        />
      </div>
    </div>
  </div>
</template>

<script>
import countries from "../assets/data/citizenships.json";

export default {
  data() {
    return {
      formData: {
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
      },
    };
  },
  methods: {},
};
</script>

<style scoped>
.form {
}
</style>