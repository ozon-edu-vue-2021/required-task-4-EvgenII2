<template>
  <form class="form" autocomplete="off" v-on:submit.prevent="formSubmit">
    <h2>Личные данные</h2>
    <div class="section-personal-data">
      <BaseInput
        id="lastname"
        label="Фамилия"
        error="Заполните поле кириллицей"
        placeholder="Введите фамилию"
        :isShowError="isShowLastnameError"
        v-on:update="formData.lastname = $event"
        v-on:blurHandler="checkLastname"
      />
      <BaseInput
        id="firstname"
        label="Имя"
        error="Заполните поле кириллицей"
        placeholder="Введите имя"
        :isShowError="isShowFirstnameError"
        v-on:update="formData.firstname = $event"
        v-on:blurHandler="checkFirstname"
      />
      <BaseInput
        id="patronymic"
        label="Отчество"
        error="Заполните поле кириллицей"
        placeholder="Введите отчество"
        :isShowError="isShowPatronymicError"
        v-on:update="formData.patronymic = $event"
        v-on:blurHandler="checkPatronymic"
      />
      <BaseInput
        id="birthday"
        label="Дата рождения"
        error="Дата должна быть ранее сегодняшней"
        type="date"
        placeholder="Введите дату"
        :isShowError="isShowBirthdayError"
        v-on:update="formData.birthday = $event"
        v-on:blurHandler="checkBirthday"
      />
      <BaseInput
        id="email"
        label="Email"
        error="Введите корректный e-mail"
        type="email"
        placeholder="Введите email"
        :isShowError="isShowEmailError"
        v-on:update="formData.email = $event"
        v-on:blurHandler="checkEmail"
      />

      <div class="country-selector" v-click-outside="hideDropdown">
        <label for="countries"> Гражданство </label>
        <input
          id="countries"
          v-model="searchCountry"
          class="form-input"
          @focus="isDropdownNationalityOpen = true"
        />
        <div
          v-if="isDropdownNationalityOpen"
          class="country-selector__dropdown"
        >
          <ul
            v-if="allCountries.length"
            class="country-selector__dropdown-items"
          >
            <li
              class="country-selector__dropdown-item"
              v-for="country in allCountries"
              :key="country.id"
              @click="onNationalityClick(country.nationality)"
            >
              {{ country.nationality }}
            </li>
          </ul>
          <div v-else class="empty">Ничего не найдено</div>
        </div>
      </div>
      <div>
        <p class="form-label">Пол</p>
        <input type="radio" id="isMan" value="true" v-model="formData.isMan" />
        <label for="isMan">Мужской</label>
        <input
          type="radio"
          id="isWoman"
          value="false"
          v-model="formData.isMan"
        />
        <label for="isWoman">Женский</label>
      </div>
      <div>
        <p class="form-label">Меняли ли фамилию или имя</p>
        <input
          type="radio"
          id="changedFirstnameOrLastname"
          value="true"
          v-model="formData.hasChangedFirstnameOrLastname"
        />
        <label for="changedFirstnameOrLastname">Да</label>
        <input
          type="radio"
          id="notChangedFirstnameOrLastname"
          value="false"
          v-model="formData.hasChangedFirstnameOrLastname"
        />
        <label for="notChangedFirstnameOrLastname">нет</label>
      </div>
      <BaseInput
        v-if="formData.hasChangedFirstnameOrLastname === 'true'"
        id="oldLastname"
        label="Фамилия"
        error="Заполните поле кириллицей"
        placeholder="Введите фамилию"
        :isShowError="isShowOldLastnameError"
        v-on:update="formData.oldLastname = $event"
        v-on:blurHandler="checkOldLastname"
      />
      <BaseInput
        v-if="formData.hasChangedFirstnameOrLastname === 'true'"
        id="oldFirstname"
        label="Имя"
        error="Заполните поле кириллицей"
        placeholder="Введите имя"
        :isShowError="isShowOldFirstnameError"
        v-on:update="formData.oldFirstname = $event"
        v-on:blurHandler="checkOldFirstname"
      />
    </div>
    <div v-if="formData.nationality === 'Russia'">
      <h2>Паспорта данные</h2>
      <div class="section-pasport-data">
        <BaseInput
          id="rusPasportSeries"
          label="Серия паспорта"
          error="Введите 4 цифры"
          placeholder="Введите серию паспорта"
          :isShowError="isShowRusPasportSeriesError"
          v-on:update="formData.rusPasportSeries = $event"
          v-on:blurHandler="checkRusPasportSeries"
        />
        <BaseInput
          id="rusPasportNumber"
          label="Номер паспорта"
          error="Введите 6 цифры"
          placeholder="Введите номер паспорта"
          :isShowError="isShowRusPasportNumberError"
          v-on:update="formData.rusPasportNumber = $event"
          v-on:blurHandler="checkRusPasportNumber"
        />
        <BaseInput
          id="rusPasportDate"
          label="Дата выдачи паспорта"
          error="Дата должна быть ранее сегодняшней"
          type="date"
          placeholder="Введите дату выдачи"
          :isShowError="isShowRusPasportDateError"
          v-on:update="formData.rusPasportDate = $event"
          v-on:blurHandler="checkRusPasportDate"
        />
      </div>
    </div>
    <div v-else-if="formData.nationality !== ''">
      <div>
        <h2>Данные о чужеземце</h2>
        <div class="section-foreigner-data">
          <BaseInput
            id="foreignLastname"
            label="Фамилия на латинице"
            error="Заполните поле латиницей"
            placeholder="Введите фамилию на латинице"
            :isShowError="isShowForeignLastnameError"
            v-on:update="formData.foreignLastname = $event"
            v-on:blurHandler="checkForeignLastname"
          />
          <BaseInput
            id="foreignFirstname"
            label="Имя на латинице"
            error="Заполните поле латиницей"
            placeholder="Введите имя на латинице"
            :isShowError="isShowForeignFirstnameError"
            v-on:update="formData.foreignFirstname = $event"
            v-on:blurHandler="checkForeignFirstname"
          />
          <BaseInput
            id="foreignPasportNumber"
            label="Номер паспорта"
            placeholder="Введите номер паспорта"
            v-on:update="formData.foreignPasportNumber = $event"
          />
          <div class="country-selector" v-click-outside="hideDropdownForeign">
            <label for="foreignPasportCountry"> Страна выдачи </label>
            <input
              id="foreignPasportCountry"
              v-model="searchCountryForeign"
              class="form-input"
              @focus="isDropdownCountriesOpen = true"
            />

            <div
              v-if="isDropdownCountriesOpen"
              class="country-selector__dropdown"
            >
              <ul
                v-if="allCountriesWithoutRussia.length"
                class="country-selector__dropdown-items"
              >
                <li
                  class="country-selector__dropdown-item"
                  v-for="country in allCountriesWithoutRussia"
                  :key="country.id"
                  @click="onForeignNationalityClick(country.nationality)"
                >
                  {{ country.nationality }}
                </li>
              </ul>
              <div v-else class="empty">Ничего не найдено</div>
            </div>
          </div>
          <div>
            <div class="pasport-selector" v-click-outside="hideDropdownPasport">
              <label for="foreignPasportType"> Тип паспорта </label>
              <input
                id="foreignPasportType"
                v-model="formData.foreignPasportType"
                class="form-input"
                @focus="isDropdownPasportTypeOpen = true"
              />
              <div
                v-if="isDropdownPasportTypeOpen"
                class="pasport-selector__dropdown"
              >
                <ul
                  v-if="pasportTypes.length"
                  class="pasport-selector__dropdown-items"
                >
                  <li
                    class="pasport-selector__dropdown-item"
                    v-for="pasport in pasportTypes"
                    :key="pasport.id"
                    @click="onPasportClick(pasport.type)"
                  >
                    {{ pasport.type }}
                  </li>
                </ul>
                <div v-else class="empty">Ничего не найдено</div>
              </div>
            </div>
            <h3>Тип паспорта</h3>
            <select v-model="formData.foreignPasportType" class="form-input">
              <option
                v-for="(pasport, id) in pasportTypes"
                :key="id"
                v-bind:value="pasport.type"
              >
                {{ pasport.type }}
              </option>
            </select>
          </div>
        </div>
      </div>
    </div>
    <button type="submit" class="form-submit-button">Отправить</button>
  </form>
</template>

<script>
// TODO
// Сделать компонент базовый инпут
// Сделать компонент базовый селект
// Сделать неактивной кнопку при неверной валидации

import countries from "../assets/data/citizenships.json";
import pasports from "../assets/data/passport-types.json";
import ClickOutside from "vue-click-outside";
import { debounce } from "../helpers/debounce";
import { regexCyrillic, regexEmail, regexLatin } from "../helpers/regex";
import BaseInput from "./BaseInput/BaseInput.vue";

const PASPORT_NUMBER = 6;
const PASPORT_SERIES = 4;

export default {
  directives: {
    ClickOutside,
  },
  components: {
    BaseInput,
  },
  created() {
    this.debouncedSearchCountry = debounce(this.getCountry, 2000);
    this.debouncedSearchCountryForeign = debounce(this.getCountryForeign, 2000);
  },
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
      },
      isDropdownNationalityOpen: false,
      isDropdownCountriesOpen: false,
      isDropdownPasportTypeOpen: false,

      searchCountry: "",
      searchCountryForeign: "",
      debouncedSearchCountry: null,
      debouncedSearchCountryForeign: null,

      allCountries: countries,
      allCountriesWithoutRussia: countries.filter(
        (country) => country.nationality !== "Russia"
      ),
      pasportTypes: pasports,

      isLastnameChecked: false,
      isShowLastnameError: false,

      isFirstnameChecked: false,
      isShowFirstnameError: false,

      isPatronymicChecked: false,
      isShowPatronymicError: false,

      isBirthdayChecked: false,
      isShowBirthdayError: false,

      isEmailChecked: false,
      isShowEmailError: false,

      isRusPasportSeriesChecked: false,
      isShowRusPasportSeriesError: false,

      isRusPasportNumberChecked: false,
      isShowRusPasportNumberError: false,

      isRusPasportDateChecked: false,
      isShowRusPasportDateError: false,

      isOldFirstnameChecked: false,
      isShowOldFirstnameError: false,

      isRusOldLastnameChecked: false,
      isShowOldLastnameError: false,

      isForeignFirstnameChecked: false,
      isShowForeignFirstnameError: false,

      isForeignLastnameChecked: false,
      isShowForeignLastnameError: false,

      isForeignPasportTypeChecked: false,
      isShowForeignPasportTypeError: false,

      isForeignPasportNumberChecked: false,
      isShowForeignPasportNumberError: false,

      isForeignPasportCountryChecked: false,
      isShowForeignPasportCountryError: false,
    };
  },
  watch: {
    searchCountry(newValue) {
      this.debouncedSearchCountry(newValue);
    },
    searchCountryForeign(newValue) {
      this.debouncedSearchCountryForeign(newValue);
    },
  },

  methods: {
    getCountry(searchWord) {
      this.allCountries = countries.filter((country) =>
        country.nationality.includes(searchWord)
      );
    },
    getCountryForeign(searchWord) {
      this.allCountriesWithoutRussia = countries.filter((country) =>
        country.nationality.includes(searchWord)
      );
    },
    onNationalityClick(selectedNationality) {
      this.formData.nationality = selectedNationality;
      this.searchCountry = selectedNationality;
      this.isDropdownNationalityOpen = false;
    },
    onForeignNationalityClick(selectedNationality) {
      this.formData.foreignPasportCountry = selectedNationality;
      this.searchCountryForeign = selectedNationality;
      this.isDropdownCountriesOpen = false;
    },
    onPasportClick(selectedType) {
      this.formData.foreignPasportType = selectedType;
      this.isDropdownPasportTypeOpen = false;
    },
    hideDropdown() {
      this.isDropdownNationalityOpen = false;
    },
    hideDropdownForeign() {
      this.isDropdownCountriesOpen = false;
    },
    hideDropdownPasport() {
      this.isDropdownPasportTypeOpen = false;
    },
    formSubmit() {
      if (this.checkForm()) console.log(this.formData);
    },
    checkForm() {
      return (
        this.isEmailChecked &&
        this.isFirstnameChecked &&
        this.isLastnameChecked &&
        this.isPatronymicChecked &&
        this.isBirthdayChecked
      );
    },
    checkLastname() {
      console.log(this.formData.lastname);
      if (
        this.formData.lastname.length > 0 &&
        this.formData.lastname.match(regexCyrillic)
      ) {
        this.isLastnameChecked = true;
        this.isShowLastnameError = false;
      } else {
        this.isLastnameChecked = false;
        this.isShowLastnameError = true;
      }
    },
    checkFirstname() {
      if (
        this.formData.firstname.length > 0 &&
        this.formData.firstname.match(regexCyrillic)
      ) {
        this.isFirstnameChecked = true;
        this.isShowFirstnameError = false;
      } else {
        this.isFirstnameChecked = false;
        this.isShowFirstnameError = true;
      }
    },
    checkPatronymic() {
      if (
        this.formData.lastname.length > 0 &&
        this.formData.lastname.match(regexCyrillic)
      ) {
        this.isPatronymicChecked = true;
        this.isShowPatronymicError = false;
      } else {
        this.isPatronymicChecked = false;
        this.isShowPatronymicError = true;
      }
    },
    checkOldLastname() {
      if (
        this.formData.oldLastname?.length > 0 &&
        this.formData.oldLastname.match(regexCyrillic)
      ) {
        this.isOldLastnameChecked = true;
        this.isShowOldLastnameError = false;
      } else {
        this.isOldLastnameChecked = false;
        this.isShowOldLastnameError = true;
      }
    },
    checkOldFirstname() {
      if (
        this.formData.oldFirstname?.length > 0 &&
        this.formData.oldFirstname.match(regexCyrillic)
      ) {
        this.isOldFirstnameChecked = true;
        this.isShowOldFirstnameError = false;
      } else {
        this.isOldFirstnameChecked = false;
        this.isShowOldFirstnameError = true;
      }
    },
    checkForeignFirstname() {
      if (
        this.formData.foreignFirstname?.length > 0 &&
        this.formData.foreignFirstname.match(regexLatin)
      ) {
        this.isForeignFirstnameChecked = true;
        this.isShowForeignFirstnameError = false;
      } else {
        this.isForeignFirstnameChecked = false;
        this.isShowForeignFirstnameError = true;
      }
    },
    checkForeignLastname() {
      if (
        this.formData.foreignLastname?.length > 0 &&
        this.formData.foreignLastname.match(regexLatin)
      ) {
        this.isForeignLastnameChecked = true;
        this.isShowForeignLastnameError = false;
      } else {
        this.isForeignLastnameChecked = false;
        this.isShowForeignLastnameError = true;
      }
    },
    checkEmail() {
      if (
        this.formData.email.length > 0 &&
        this.formData.email.match(regexEmail)
      ) {
        this.isEmailChecked = true;
        this.isShowEmailError = false;
      } else {
        this.isEmailChecked = false;
        this.isShowEmailError = true;
      }
    },
    checkRusPasportSeries() {
      if (
        this.formData.rusPasportSeries?.length === PASPORT_SERIES &&
        !isNaN(this.formData.rusPasportSeries)
      ) {
        this.isRusPasportSeriesChecked = true;
        this.isShowRusPasportSeriesError = false;
      } else {
        this.isRusPasportSeriesChecked = false;
        this.isShowRusPasportSeriesError = true;
      }
    },
    checkRusPasportNumber() {
      console.log(this.formData.rusPasportNumber);
      if (
        this.formData.rusPasportNumber?.length === PASPORT_NUMBER &&
        !isNaN(this.formData.rusPasportNumber)
      ) {
        this.isRusPasportNumberChecked = true;
        this.isShowRusPasportNumberError = false;
      } else {
        this.isRusPasportNumberChecked = false;
        this.isShowRusPasportNumberError = true;
      }
    },
    checkRusPasportDate() {
      if (
        this.formData.rusPasportDate &&
        new Date(this.formData.rusPasportDate) < new Date()
      ) {
        this.isRusPasportDateChecked = true;
        this.isShowRusPasportDateError = false;
      } else {
        this.isRusPasportDateChecked = false;
        this.isShowRusPasportDateError = true;
      }
    },
    checkBirthday() {
      if (new Date(this.formData.birthday) < new Date()) {
        this.isBirthdayChecked = true;
        this.isShowBirthdayError = false;
      } else {
        this.isBirthdayChecked = false;
        this.isShowBirthdayError = true;
      }
    },
  },
};
</script>

<style scoped>
.form {
  text-align: center;
}
.section-personal-data {
  border: 1px solid black;
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 100px;
  row-gap: 20px;
  padding: 50px;
}
.form-input {
  width: 100%;
  font-size: 16px;
  border: 0;
  outline: 0;
  background: transparent;
  border-bottom: 1px solid grey;
}
.form-label {
  padding: 0;
  margin: 0 0 20px;
  font-size: 18px;
}
.section-pasport-data {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid black;
  column-gap: 50px;
  padding: 30px 50px;
}
.section-foreigner-data {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  border: 1px solid black;
  column-gap: 50px;
  padding: 30px 50px;
}
.form-submit-button {
  font-size: 16px;
  margin-top: 30px;
  padding: 10px;
}
.country-selector {
  position: relative;
}
.country-selector__dropdown {
  position: absolute;
  background: white;
  width: 100%;
  text-align: left;
}
.country-selector__dropdown-items {
  height: 150px;
  overflow-y: scroll;
}
.country-selector__dropdown-item {
  cursor: pointer;
  list-style: none;
  margin-bottom: 5px;
}
.pasport-selector {
  position: relative;
}
.pasport-selector__dropdown {
  position: absolute;
  background: white;
  width: 100%;
  text-align: left;
}
.pasport-selector__dropdown-item {
  cursor: pointer;
  list-style: none;
  margin-bottom: 5px;
}
</style>
