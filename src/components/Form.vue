<template>
  <div class="form">
    <div class="row">
    </div>
    <div class="personalData">
      <h2>Личные данные</h2>
      <div class="row">
        <label v-bind:class="{ error: !formCheck.lastName }">
          <p class="">Фамилия</p>
          <input class="input-text" type="text" v-model="formData.lastName" @focus="formCheck.lastName = true" />
          <p class="text_error" v-if="!formCheck.lastName">В тексте должна быть только кирилца</p>
        </label>
        <label v-bind:class="{ error: !formCheck.firstName }" >
          <p>Имя</p>
          <input 
            class="input-text"
            type="text"
            v-model="formData.firstName"
            @focus="formCheck.firstName = true"
          />
          <p class="text_error" v-if="!formCheck.firstName">В тексте должна быть только кирилца</p>
        </label>
        <label v-bind:class="{ error: !formCheck.patronymic }">
          <p>Отчество</p>
          <input
            class="input-text"
            type="text"
            v-model="formData.patronymic"
            @focus="formCheck.patronymic = true"
          />
          <p class="text_error" v-if="!formCheck.patronymic">В тексте должна быть только кирилца</p>
        </label>
      </div>
      <div class="row">
        <label v-bind:class="{ error: !formCheck.dateOfBirth }">
          <p>Дата рождения</p>
          <input
            class="input-text"
            type="date"
            v-model="formData.dateOfBirth"
            @focus="formCheck.dateOfBirth = true"
          />
          <p class="text_error" v-if="!formCheck.dateOfBirth">Дата невалидная</p>
        </label>
      </div>
      <div class="row">
        <label v-bind:class="{ error: !formCheck.email }">
          <p>E-mail</p>
          <input
            class="input-text"
            type="email"
            v-model="formData.email"
            @focus="formCheck.email = true"
          />
          <p class="text_error" v-if="!formCheck.email">E-mail невалидный</p>
        </label>
      </div>
    </div>
    <div class="gender">
      <h2>Пол</h2>
      <label>
        <input type="radio" name="gender" v-model="formData.gender" value="man" />
        Мужской
      </label>
      <label>
        <input type="radio" name="gender" v-model="formData.gender" value="woman" />
        Женский
      </label>
    </div>
    <div class="passportData">
      <div class="row"
        v-click-outside="hideDropdown"
      >
        <label v-bind:class="{ error: !formCheck.citizenship }">
          Гражданство
          <input 
            class="input-text" type="text"
            v-model="searchWord"
            @focus="isDropdownOpenContry = true, formCheck.citizenship = true"
            @keydown.enter="hideDropdown()"
          />
          <p class="text_error" v-if="!formCheck.citizenship">Выберите страну</p>
          <div
            v-if="isDropdownOpenContry"
          >
            <ul 
              class="list-country"
              v-if="allCitizens.length"
            >
              <li v-if="!filterCitizens.length">
                Список пуст
              </li>
              <li
                v-for="country in filterCitizens"
                :key="country.id"
                @click="onCountryClicked(1, country.nationality)"
              >
                {{ country.nationality }}
              </li>
            </ul>
            <div v-else>Список пуст</div>
          </div>
        </label>
      </div>
      <div
        class="row"
        v-if="formData.citizenship === 'Russia' ||
          formData.citizenship === 'russia'"
      >
        <label  v-bind:class="{ error: !formCheck.series }">
          <p>Серия паспорта</p>
          <input class="input-text" type="text" v-model="formData.series" />
          <p class="text_error" v-if="!formCheck.series">Серия должна содержать 4 цифры</p>
        </label>
        <label  v-bind:class="{ error: !formCheck.number }">
          <p>Номер паспота</p>
          <input class="input-text" type="text" v-model="formData.number" />
          <p class="text_error" v-if="!formCheck.number">Номер должен содержать 6 цифр</p>
        </label>
        <label v-bind:class="{ error: !formCheck.dateOfIssue }">
          <p>Дата выдачи</p>
          <input class="input-text" type="date" v-model="formData.dateOfIssue" />
          <p class="text_error" v-if="!formCheck.dateOfIssue">Дата не валидна</p>
        </label>
      </div>
      <div
        class="column"
        v-else-if="filterCitizens.length &&
          filterCitizens[0].nationality !== 'Russia' &&
          formData.citizenship !== 'russia' &&
          formData.citizenship.length !== 0"
      >
        <div class="row">
          <label v-bind:class="{ error: !formCheck.lastNameLat }">
            <p>Фамилия на латинице</p>
            <input
              class="input-text"
              type="text"
              v-model="formData.lastNameLat"
              @focus="formCheck.lastNameLat = true"
            />
            <p class="text_error" v-if="!formCheck.lastNameLat">В тексте должна быть только латиница</p>
          </label>
          <label  v-bind:class="{ error: !formCheck.firstNameLat }">
            <p>Имя на латинице</p>
            <input
              class="input-text"
              type="text"
              v-model="formData.firstNameLat"
              @focus="formCheck.firstNameLat = true"
            />
            <p class="text_error" v-if="!formCheck.firstNameLat">В тексте должна быть только латиница</p>
          </label>
        </div>
        <div class="row">
          <label v-bind:class="{ error: !formCheck.numberLat }">
            <p>Номер паспота</p>
            <input
              class="input-text"
              type="text"
              v-model="formData.numberLat"
              @focus="formCheck.numberLat = true"
            />
            <p class="text_error" v-if="!formCheck.numberLat">Номер должен содержать 6 цифр</p>
          </label>
          <label>
            <p>Страна выдачи</p>
            <input 
            class="input-text" type="text"
            v-model="formData.countryOfExpiration"
            @focus="isDropdownOpenExpiration = true"
            @keydown.enter="hideDropdown(2)"
          />
          </label>
          <label v-bind:class="{ error: !formCheck.typePassport }">
            <p>Тип паспорта</p>
            <input
              class="input-text"
              type="input"
              v-model="formData.typePassport"
              @focus="formCheck.typePassport = true"
            />
          </label>
        </div>
      </div>
    </div>
    <div class="changeId">
      <h2>Меняли ли фамилию или имя?</h2>
      <div class="row">
        <label>
          <input class="input-radio" type="radio" name="changeId" v-model="formData.changeId" value="0" />
          Нет
        </label>
        <label>
          <input class="input-radio" type="radio" name="changeId" v-model="formData.changeId" value="1" />
          Да
        </label>
      </div>
      <div
        class="row"
        v-if="formData.changeId === '1'"
      >
        <label v-bind:class="{ error: !formCheck.originLastName }">
          <p>Предыдущая Фамилия</p>
          <input
            class="input-text"
            type="text"
            v-model="formData.originLastName"
            @focus="formCheck.originLastName = true"  
          />
          <p class="text_error" v-if="!formCheck.originLastName">В тексте должна быть только кирилца</p>
        </label>
        <label v-bind:class="{ error: !formCheck.originFirstName }">
          <p>Предыдущее Имя</p>
          <input
            class="input-text"
            type="text"
            v-model="formData.originFirstName"
            @focus="formCheck.originFirstName = true"
          />
          <p class="text_error" v-if="!formCheck.originFirstName">В тексте должна быть только кирилца</p>
        </label>
      </div>
    </div>
    <div class="row row-button">
      <button class="button" @click="formSubmit">Отправить</button>
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import citizenships from "@/assets/data/citizenships.json";
import { debounce } from "@/utils/debounce.js";

export default {
  directives: {
    ClickOutside,
  },
  data() {
    return {
      formData: {
        lastName: "",
        firstName: "",
        patronymic: "",
        dateOfBirth: "",
        email: "",
        gender: "",
        citizenship: "",
        series: "",
        number: "",
        dateOfIssue: "",
        changeId: -1,
        originLastName: "",
        originFirstName: "",
        numberLat: "",
        lastNameLat: "",
        firstNameLat: "",
        countryOfExpiration: "",
        typePassport: "",
      },
      formCheck: {
        lastName: true,
        firstName: true,
        patronymic: true,
        dateOfBirth: true,
        email: true,
        series: true,
        number: true,
        citizenship: true,
        lastNameLat: true,
        firstNameLat: true,
        numberLat: true,
        dateOfIssue: true,
        typePassport: true,
        originLastName: true,
        originFirstName: true,
      },
      isDropdownOpenContry: false,
      isDropdownOpenExpiration: false,
      allCitizens: citizenships,
      filterCitizens: citizenships,
      searchWord: "",
      debouncedSearchCitizens: null,
    };
  },
  created() {
    this.debouncedSearchCitizens = debounce(this.getCitizens, 1000);
  },
  methods: {
    check() {
      this.formCheck.lastName = this.checkRu(this.formData.lastName);
      this.formCheck.firstName = this.checkRu(this.formData.firstName);
      this.formCheck.patronymic = this.checkRu(this.formData.patronymic);
      this.formCheck.dateOfBirth = this.checkDate(this.formData.dateOfBirth);
      this.formCheck.email = this.checkEmail(this.formData.email);
      if(!this.formData.citizenship.length) {
        this.formCheck.citizenship = false;
      }
      if(this.formData.citizenship === "russia" ||
        this.formData.citizenship === "Russia") {
        this.formCheck.series = this.checkNum(4, this.formData.series);
        this.formCheck.number = this.checkNum(6, this.formData.number);
        this.formCheck.dateOfIssue = this.checkDate(this.formData.dateOfIssue);
      } else if(this.formData.citizenship) {
        this.formCheck.firstNameLat = this.checkLat(this.formData.firstNameLat);
        this.formCheck.lastNameLat = this.checkLat(this.formData.lastNameLat);
        this.formCheck.numberLat = this.checkNum(6, this.formData.numberLat);
        this.formCheck.typePassport = this.checkRu(this.formData.typePassport);
      }
      if(this.formData.changeId === '1') {
        this.formCheck.originLastName = this.checkRu(this.formData.originLastName);
        this.formCheck.originFirstName = this.checkRu(this.formData.originFirstName);
      }
      if(!this.formData.citizenship.length) {
        this.formCheck.citizenship = false;
      } else {
        this.formCheck.citizenship = true;
      }
    },
    checkRu(str) {
      let re = /^[А-ЯЁа-яё]+$/;
			return re.test(String(str).toLowerCase());
    },
    checkLat(str) {
      if(this.formData.citizenship != "russia" &&
        this.formData.citizenship) {
        let re = /^[A-Za-z]+$/;
        return re.test(String(str).toLowerCase());
      }
      return true;
    },
    checkNum(num, str) {
      if(num === str.length) {
        let re = /^\d+$/;
        return re.test(Number(str));
      }
      return false;
    },
    checkDate(str) {
      if(str.length != 0) {
        let d = new Date();
        let year = d.getFullYear();
        let month = d.getMonth() + 1;
        let day = d.getDate();
        if(day < 10) {
          day = "0" + day;
        }
        let date = year + "-" + month + "-" + day;
        if(str <= date) {
          return true;
        }
      }
      return false;
    },
    checkEmail(str) {
      let re = /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/;
			return re.test(String(str).toLowerCase());
    },
    hideDropdown() {
      this.isDropdownOpenContry = false;
    },
    onCountryClicked(num, selectedCountry) {
      this.searchWord = selectedCountry;
      this.formData.citizenship = this.searchWord;
      this.hideDropdown();
    },
    formSubmit() {
      this.check();
      for(let k in this.formCheck) {
        if(this.formCheck[k] === false) {
          console.warn("the form cannot be sent to server");
          return;
        }
      }
      console.log(this.formData);
    },
    getCitizens(searchWord) {
      this.filterCitizens = this.allCitizens.filter((citi) => {
        this.formData.citizenship = searchWord;
        return citi.nationality.toLowerCase().includes(searchWord.toLowerCase());
      });
    },
  },
  watch: {
    searchWord(newValue) {
      this.debouncedSearchCitizens(newValue);
    }
  }
};
</script>

<style scoped>

.form {
  display: flex;
  flex-direction: column;
  padding: 10px;
  color: rgb(111, 119, 135);
  border: 2px solid rgb(111, 119, 135);
  border-radius: 3px;
}
.row {
  display: flex;
}
.column {
  display: flex;
  flex-direction: column;
}
.input-text {
  padding: 5px 15px;
  background: rgb(28, 30, 33);
  color: rgb(111, 119, 135);
  height: 45px;
  width: 220px;
  border: none;
  border-radius: 3px;
  outline:none;
}
.error {
  color: rgb(99, 2, 18);
}
.error .input-text {
  border: 1px solid rgb(99, 2, 18);
  color: rgb(99, 2, 18);
}
h2 {
  margin: 10px;
}
label {
  margin: 10px;
}
label .input-text{
  display: flex;
}
.form input:focus {
  color: rgb(255, 255, 255);
}
.form button {
  height: 45px;
  width: 320px;
  background: rgb(28, 30, 33);
  color: rgb(21, 112, 187);
  border: none;
  border-radius: 3px;
}
.form button:hover {
  cursor: pointer;
}
.list-country {
  overflow: auto;
  max-height: 400px;
  background-color: rgb(28, 30, 33);
  border-radius: 3px;
  padding: 10px;
  text-overflow: ellipsis;
}
.list-country li {
  height: 45px;
  list-style-type: none;
  cursor: pointer;
  display: flex;
  align-items: center;
}
.row-button {
  justify-content: center;
}
.text_error {
  margin: 3px 0 0 0;
  font-size: 12px;
}
</style>