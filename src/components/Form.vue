<template>
  <div class="form">
    <div class="row">
        {{ formData }}
    </div>
    <div class="personalData">
      <h2>Личные данные</h2>
      <div class="row">
        <label>
          Фамилия
          <input class="input-text" type="text" v-model="formData.lastName" />
        </label>
        <label>
          Имя
          <input class="input-text" type="text" v-model="formData.firstName" />
        </label>
        <label>
          Отчество
          <input class="input-text" type="text" v-model="formData.patronymic" />
        </label>
      </div>
      <div class="row">
        <label>
          Дата рождения
          <input class="input-text" type="date" v-model="formData.dateOfBirth" />
        </label>
      </div>
      <div class="row">
        <label>
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
            v-model="formData.citizenship"
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
        v-if="formData.citizenship == 'Russia'"
      >
        <label>
          Серия паспорта
          <input class="input-text" type="text" v-model="formData.series" />
        </label>
        <label>
          Номер паспота
          <input class="input-text" type="text" v-model="formData.number" />
        </label>
        <label>
          Дата выдачи
          <input class="input-text" type="date" v-model="formData.dateOfIssue" />
        </label>
      </div>
      <div
        class="row"
        v-else
      >
        <div class="row">
          <label>
            Фамилия на латинице
            <input class="input-text" type="text" v-model="formData.lastNameLat" />
          </label>
          <label>
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
        <label>Фамилия
          <input class="input-text" type="text" v-model="formData.originLastName" />
        </label>
        <label>Имя
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
      isDropdownOpenContry: false,
      isDropdownOpenExpiration: false,
      allCitizens: citizenships,
    };
  },
  methods: {
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
      console.log("update", this.formData);
    }
  },
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

// if(phone) {
// 			let re = /\+7\s\(\d{3}\)\s\d{3}-\d{2}-\d{2}/;
// 			this.state.phone = re.test(phone);
// 		}
// 		if(email) {
// 			let re = /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/;
// 			this.state.email = re.test(String(email).toLowerCase());
// 		}