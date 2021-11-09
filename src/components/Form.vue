<template>
  <div class="form">
    <div class="row">
    </div>
    <div class="personalData">
      <h2>Личные данные</h2>
      <div class="row">
        <label v-bind:class="{ error: !formCheck.lastName }">
          Фамилия
          {{ formCheck.lastName }}
          <input class="input-text" type="text" v-model="formData.lastName" />
        </label>
        <label v-bind:class="{ error: !formCheck.firstName }" >
          Имя
          <input class="input-text" type="text" v-model="formData.firstName" />
        </label>
        <label v-bind:class="{ error: !formCheck.patronymic }">
          Отчество
          <input class="input-text" type="text" v-model="formData.patronymic" />
        </label>
      </div>
      <div class="row">
        <label v-bind:class="{ error: !formCheck.dateOfBirth }">
          Дата рождения
          <input class="input-text" type="date" v-model="formData.dateOfBirth" />
        </label>
      </div>
      <div class="row">
        <label v-bind:class="{ error: !formCheck.email }">
          E-mail
          <input class="input-text" type="email" v-model="formData.email" />
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
        <label>
          Гражданство
          <input 
            class="input-text" type="text"
            v-model="searchWord"
            @focus="isDropdownOpenContry = true"
            @keydown.enter="hideDropdown(1)"
          />
          <div
            v-if="isDropdownOpenContry"
          >
            <ul 
              class="list-country"
              v-if="allCitizens.length"
            >
              <li
                v-for="country in allCitizens"
                :key="country.id"
                @click="onCountryClicked(1, country.nationality)"
              >
                {{ country.nationality }}
              </li>
            </ul>
              <div v-else>Отсутсвует соединение с сервером</div>
          </div>
        </label>
      </div>
      <div
        class="row"
        v-if="formData.citizenship === 'Russia'"
      >
        <label  v-bind:class="{ error: !formCheck.series }">
          Серия паспорта
          <input class="input-text" type="text" v-model="formData.series" />
        </label>
        <label  v-bind:class="{ error: !formCheck.number }">
          Номер паспота
          <input class="input-text" type="text" v-model="formData.number" />
        </label>
        <label v-bind:class="{ error: !formCheck.dateOfIssue }">
          Дата выдачи
          <input class="input-text" type="date" v-model="formData.dateOfIssue" />
        </label>
      </div>
      <div
        class="row"
        v-else-if="formData.citizenship !== 'Russia' && formData.citizenship.length !== 0"
      >
        <div class="row">
          <label v-bind:class="{ error: !formCheck.lastNameLat }">
            Фамилия на латинице
            <input class="input-text" type="text" v-model="formData.lastNameLat" />
          </label>
          <label  v-bind:class="{ error: !formCheck.firstNameLat }">
            Имя на латинице
            <input class="input-text" type="text" v-model="formData.firstNameLat" />
          </label>
        </div>
        <div class="row">
          <label>
            Номер паспота
            <input class="input-text" type="text" v-model="formData.number" />
          </label>
          <label>
            Страна выдачи
            <input 
            class="input-text" type="text"
            v-model="formData.countryOfExpiration"
            @focus="isDropdownOpenExpiration = true"
            @keydown.enter="hideDropdown(2)"
          />
          <div
            v-if="isDropdownOpenExpiration"
          >
            <ul 
              class="list-country"
              v-if="allCitizens.length"
            >
              <li
                v-for="country in allCitizens"
                :key="country.id"
                @click="onCountryClicked(2, country.nationality)"
              >
                {{ country.nationality }}
              </li>
            </ul>
            <div v-else>Отсутсвует соединение с сервером</div>
          </div>
          </label>
          <label>
            Тип паспорта
            <input class="input-text" type="date" v-model="formData.dateOfIssue" />
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
        v-if="formData.changeId == 1"
      >
        <label v-bind:class="{ error: !formCheck.originLastName }">
          Предыдущая Фамилия
          <input class="input-text" type="text" v-model="formData.originLastName" />
        </label>
        <label v-bind:class="{ error: !formCheck.originFirstName }">
          Предыдущее Имя
          <input class="input-text" type="text" v-model="formData.originFirstName" />
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
        lastNameLat: "",
        firstNameLat: "",
        countryOfExpiration: "",
      },
      formCheck: {
        lastName: true,
        firstName: true,
        patronymic: true,
        dateOfBirth: true,
        email: true,
        series: true,
        number: true,
        lastNameLat: true,
        firstNameLat: true,
        dateOfIssue: true,
        originLastName: true,
        originFirstName: true,
      },
      isDropdownOpenContry: false,
      isDropdownOpenExpiration: false,
      allCitizens: citizenships,
      filterCitizens: {},
      searchWord: "",
      debouncedSearchCitizens: null,
    };
  },
  created() {
    this.debouncedSearchCitizens = debounce(this.getCitizens, 2000);
  },
  methods: {
    check() {
      this.formCheck.lastName = this.checkRu(this.formData.lastName);
      this.formCheck.firstName = this.checkRu(this.formData.firstName);
      this.formCheck.patronymic = this.checkRu(this.formData.patronymic);
      this.formCheck.dateOfBirth = this.checkDate(this.formData.dateOfBirth);
      this.formCheck.email = this.checkEmail(this.formData.email);
      if(this.formData.citizenship == "russia") {
        this.formCheck.series = this.checkNum(4, this.formData.series);
        this.formCheck.number = this.checkNum(6, this.formData.number);
        this.formCheck.dateOfIssue = this.checkDate(this.formData.dateOfIssue);
      } else if(this.formData.citizenship) {
        this.formCheck.firstNameLat = this.checkLat(this.formData.firstNameLat);
        this.formCheck.lastNameLat = this.checkLat(this.formData.lastNameLat);
      }
      if(this.formData.changeId == 1) {
        this.formCheck.originLastName = this.checkRu(this.formData.originLastName);
        this.formCheck.originFirstName = this.checkRu(this.formData.originFirstName);
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
      if(num == str.length) {
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
    hideDropdown(num) {
      if(num == 1) {
        this.isDropdownOpenContry = false;
      } else if(num == 2) {
        this. isDropdownOpenExpiration = false;
      }
    },
    onCountryClicked(num, selectedCountry) {
      if(num == 1) {
        this.formData.citizenship = selectedCountry;
      } else if(num == 2) {
        this.formData.countryOfExpiration = selectedCountry;
      }
      this.hideDropdown(num);
    },
    formSubmit() {
      this.check();
      for(let k in this.formCheck) {
        console.log(k, this.formCheck[k]);
        if(this.formCheck[k] == false) {
          console.warn("the form cannot be sent to server");
          return;
        }
      }
      console.log("update", this.formData);
    },
    getCitizens(searchWord) {
      console.log("Getch citizens: ", searchWord);
      // return this.allCitizens.filter(citizens => citizens.nationality == searchWord)
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
  color: rgb(170, 2, 30);
}

.error .input-text {
  border: 1px solid rgb(170, 2, 30);
  color: rgb(170, 2, 30);
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
  height: 400px;
  background-color: rgb(28, 30, 33);
  border-radius: 3px;
  padding: 10px;
  text-overflow: ellipsis;
  overflow: hidden;
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
</style>