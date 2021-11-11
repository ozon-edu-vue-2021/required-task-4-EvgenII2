<template>
  <form class="form" autocomplete="off" v-on:submit.prevent="formSubmit">
    <h2>Личные данные</h2>
    <div class="section-personal-data">
      <div>
        <label for="lastname" class="form-label">
          Фамилия {{ formData.email }}
        </label>
        <input
          id="lastname"
          class="form-input"
          type="text"
          v-model="formData.lastname"
          placeholder="Введите фамилию"
          autofocus
          required
          @blur="checkLastname"
        />
        <p v-if="isShowLastnameError" class="input-error">
          Заполните поле кириллицей
        </p>
      </div>
      <div>
        <label for="firstname" class="form-label"> Имя </label>
        <input
          id="firstname"
          class="form-input"
          type="text"
          v-model="formData.firstname"
          placeholder="Введите имя"
          autofocus
          required
          @blur="checkFirstname"
        />
        <p v-if="isShowFirstnameError" class="input-error">
          Заполните поле кириллицей
        </p>
      </div>
      <div>
        <label for="patronymic" class="form-label">Отчество</label>
        <input
          id="patronymic"
          class="form-input"
          type="text"
          v-model="formData.patronymic"
          placeholder="Введите отчество"
          autofocus
          required
          @blur="checkPatronymic"
        />
        <p v-if="isShowPatronymicError" class="input-error">
          Заполните поле кириллицей
        </p>
      </div>
      <div>
        <label for="birthday" class="form-label">Дата рождения</label>
        <input
          id="birthday"
          class="form-input"
          type="date"
          v-model="formData.birthday"
          placeholder="Введите дату"
          required
          autofocus
          @blur="checkBirthday"
        />
        <p v-if="isShowBirthdayError" class="input-error">
          Дата должна быть ранее сегодняшней
        </p>
      </div>
      <div>
        <label for="email" class="form-label">E-mail</label>
        <input
          id="email"
          class="form-input"
          type="email"
          v-model="formData.email"
          placeholder="Введите e-mail"
          required
          autofocus
          @blur="checkEmail"
        />
        <p v-if="isShowEmailError" class="input-error">
          Введите корректный e-mail
        </p>
      </div>
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
      <div v-if="formData.hasChangedFirstnameOrLastname === 'true'">
        <label for="oldLastname" class="form-label">Фамилия</label>
        <input
          id="oldLastname"
          class="form-input"
          type="text"
          v-model="formData.oldLastname"
          placeholder="Введите фамилию"
          autofocus
          @blur="checkOldLastname"
        />
        <p v-if="isShowOldLastnameError">Заполните поле кириллицей</p>
      </div>
      <div v-if="formData.hasChangedFirstnameOrLastname === 'true'">
        <label for="oldFirstname" class="form-label">Имя</label>
        <input
          id="oldFirstname"
          class="form-input"
          type="text"
          v-model="formData.oldFirstname"
          placeholder="Введите имя"
          autofocus
          @blur="checkOldFirstname"
        />
        <p v-if="isShowOldFirstnameError">Заполните поле кириллицей</p>
      </div>
    </div>
    <div v-if="formData.nationality === 'Russia'">
      <h2>Паспорта данные</h2>
      <div class="section-pasport-data">
        <div>
          <label for="rusPasportSeries" class="form-label">
            Серия паспорта
          </label>
          <input
            id="rusPasportSeries"
            class="form-input"
            type="text"
            v-model="formData.rusPasportSeries"
            placeholder="Введите серию паспорта"
            autofocus
            @blur="checkRusPasportSeries"
          />
          <p v-if="isShowRusPasportSeriesError" class="input-error">
            Введите 4 цифры
          </p>
        </div>
        <div>
          <label for="rusPasportNumber" class="form-label">
            Номер паспорта
          </label>
          <input
            id="rusPasportNumber"
            class="form-input"
            type="text"
            v-model="formData.rusPasportNumber"
            placeholder="Введите номер паспорта"
            autofocus
            @blur="checkRusPasportNumber"
          />
          <p v-if="isShowRusPasportNumberError" class="input-error">
            Введите 6 цифр
          </p>
        </div>

        <div>
          <label for="rusPasportDate" class="form-label">
            Дата выдачи паспорта
          </label>
          <input
            id="rusPasportDate"
            class="form-input"
            type="date"
            v-model="formData.rusPasportDate"
            placeholder="Введите дату выдачи"
            v-bind:max="getToday"
            autofocus
            @blur="checkRusPasportDate"
          />
          <p v-if="isShowRusPasportDateError" class="input-error">
            Дата должна быть ранее сегодняшней
          </p>
        </div>
      </div>
    </div>
    <div v-else-if="formData.nationality !== ''">
      <div>
        <h2>Данные о чужеземце</h2>
        <div class="section-foreigner-data">
          <div>
            <label for="foreignLastname" class="form-label">
              Фамилия на латинице
            </label>
            <input
              id="foreignLastname"
              class="form-input"
              type="text"
              v-model="formData.foreignLastname"
              placeholder="Введите фамилию на латинице"
              autofocus
              @blur="checkForeignLastname"
            />
            <p v-if="isShowForeignLastnameError" class="input-error">
              Заполните поле латиницей
            </p>
          </div>
          <div>
            <label for="foreignFirstname" class="form-label">
              Имя на латинице
            </label>
            <input
              id="foreignFirstname"
              class="form-input"
              type="text"
              v-model="formData.foreignFirstname"
              placeholder="Введите имя на латинице"
              autofocus
              @blur="checkForeignFirstname"
            />
            <p v-if="isShowForeignFirstnameError" class="input-error">
              Заполните поле латиницей
            </p>
          </div>
          <div>
            <label for="foreignPasportNumber" class="form-label">
              Номер паспорта
            </label>
            <input
              id="foreignPasportNumber"
              class="form-input"
              type="text"
              v-model="formData.foreignPasportNumber"
              placeholder="Введите номер паспорта"
              autofocus
            />
          </div>
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

const PASPORT_NUMBER = 6;
const PASPORT_SERIES = 4;

export default {
  directives: {
    ClickOutside,
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
        isMan: "true",
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
  computed: {
    getToday: function () {
      const today = new Date();
      let dd = today.getDate();
      let mm = today.getMonth() + 1;
      const yyyy = today.getFullYear();
      if (dd < 10) {
        dd = "0" + dd;
      }
      if (mm < 10) {
        mm = "0" + mm;
      }
      return yyyy + "-" + mm + "-" + dd;
    },
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
        this.formData.oldLastname.length > 0 &&
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
        this.formData.oldFirstname.length > 0 &&
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
        this.formData.foreignFirstname.length > 0 &&
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
        this.formData.foreignLastname.length > 0 &&
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
.form-label {
  padding: 0;
  margin: 0 0 20px;
  font-size: 18px;
}
.input-error {
  color: red;
}
</style>
