<template>
  <div class="wrapper">
    <div class="container">
      <div class="client-created" v-if="isClientCreate">
        <p>Клиент успешно создан</p>
      </div>
      <form class="create-client" @submit.prevent="createClient" v-if="!isClientCreate">
        <div class="main-info">
          <span class="main-info__title">Основная информация</span>
          <div class="main-info__row">
            <!-- Фамилия -->
            <label for="surname" :class="$v.form.surname.$error ? 'invalid-label' : ''">Фамилия*</label>
            <input
              type="text"
              :class="$v.form.surname.$error ? 'invalid-input' : ''"
              id="surname"
              placeholder="Фамилия*"
              v-model.trim="form.surname"
            />
            <p
              class="error-input"
              v-if="$v.form.surname.$dirty && !$v.form.surname.required"
            >Обязательное поле</p>

            <!-- Имя -->
            <label for="name" :class="$v.form.name.$error ? 'invalid-label' : ''">Имя*</label>
            <input
              type="text"
              :class="$v.form.name.$error ? 'invalid-input' : ''"
              id="name"
              placeholder="Имя*"
              v-model.trim="form.name"
            />
            <p
              class="error-input"
              v-if="$v.form.name.$dirty && !$v.form.name.required"
            >Обязательное поле</p>

            <!-- Отчество -->
            <label for="middle-name">Отчество</label>
            <input type="text" id="middle-name" placeholder="Отчество" v-model="form.middleName" />

            <!-- Дата рождения -->
            <label
              for="birthdate"
              min="1920-01-01"
              max="2020-01-01"
              :class="$v.form.birthdate.$error ? 'invalid-label' : ''"
            >Дата рождения*</label>
            <input
              type="date"
              name="birthdate"
              id="birthdate"
              v-model="form.birthdate"
              :class="$v.form.birthdate.$error ? 'invalid-input' : ''"
            />
            <p
              class="error-input"
              v-if="$v.form.birthdate.$dirty && !$v.form.birthdate.required"
            >Обязательное поле</p>

            <!-- Номер телефона -->
            <label
              for="phone-text"
              :class="$v.form.birthdate.$error ? 'invalid-label' : ''"
            >Номер телефона*</label>
            <input
              type="text"
              id="phone-text"
              placeholder="+7 (___)___-__-__"
              v-model.trim="form.phone"
              :class="$v.form.phone.$error ? 'invalid-input' : ''"
            />
            <p
              class="error-input"
              v-if="$v.form.phone.$dirty && !$v.form.phone.required"
            >Обязательное поле</p>
            <p
              class="error-input"
              v-if="$v.form.phone.$dirty && !$v.form.phone.numeric"
            >Неправильный формат</p>

            <!-- Пол -->
            <div class="main-info__selectors-row">
              <div class="main-info__select">
                <label for="gender">Пол</label>
                <select name="gender" id="gender" v-model="form.gender">
                  <option value="Male">Мужской</option>
                  <option value="Female">Женский</option>
                </select>
              </div>

              <!-- Группа клиентов -->
              <div class="main-info__multi-select">
                <label :class="$v.form.checkedGroupClient.$error ? 'invalid-label' : ''">Группа клиентов*</label>
                <div v-for="(group,index) in groupClients" :key="index">
                  <input
                    type="checkbox"
                    :id="index"
                    :value="index"
                    v-model="form.checkedGroupClient"
                  />
                  <label :for="index">{{ group }}</label>
                </div>
                <p
                  class="error-input__group-clients"
                  v-if="$v.form.checkedGroupClient.$dirty && !$v.form.checkedGroupClient.required"
                >Обязательное поле</p>
              </div>

              <!-- Лечащий врач -->
              <div class="main-info__select">
                <label for="treating-doctor">Лечащий врач</label>
                <select name="treating-doctor" id="treating-doctor">
                  <option>Иванов</option>
                  <option>Захаров</option>
                  <option>Чернышева</option>
                </select>
              </div>
            </div>

            <!-- Не отправлять СМС -->
            <div class="main-info__checkbox">
              <label for="not-send-sms">Не отправлять СМС</label>
              <input type="checkbox" name="not-send-sms" id="not-send-sms" />
            </div>
          </div>
        </div>
        <div class="adress">
          <span class="adress__title">Адрес</span>
          <div class="adress__row">
            <!-- Индекс -->
            <div class="input-text">
              <label for="index">Индекс</label>
              <input type="text" id="index" placeholder="Индекс" />
            </div>

            <!-- Страна -->
            <div class="input-text">
              <label for="country">Страна</label>
              <input type="text" id="country" placeholder="Страна"/>
            </div>

            <!-- Область -->
            <div class="input-text">
              <label for="region">Область</label>
              <input type="text" id="region" placeholder="Область" />
            </div>

            <!-- Город -->
            <div class="input-text">
              <label for="city" :class="$v.form.city.$error ? 'invalid-label' : ''">Город*</label>
              <input type="text" id="city" placeholder="Город*" v-model.trim="form.city" :class="$v.form.city.$error ? 'invalid-input' : ''"/>
               <p
                  class="error-input"
                  v-if="$v.form.city.$dirty && !$v.form.city.required"
                >Обязательное поле</p>
            </div>
           

            <!-- Улица -->
            <div class="input-text">
              <label for="street">Улица</label>
              <input type="text" id="street" placeholder="Улица" />
            </div>

            <!-- Дом -->
            <div class="input-text">
              <label for="house">Дом</label>
              <input type="text" id="house" placeholder="Дом" />
            </div>
          </div>
        </div>
        <div class="passport">
          <span class="passport__title">Паспорт</span>
          <div class="passport__row">
            <!-- Тип документа -->
            <label for="document-type" :class="$v.form.documentType.$error ? 'invalid-label' : ''">Тип документа*</label>
            
              <div class="passport__selectors-row">
                <div class="passport__select">
                  <select name="document-type" id="document-type" v-model="form.documentType" :class="$v.form.documentType.$error ? 'invalid-input' : ''">
                    <option value="passport">Паспорт</option>
                    <option value="birth-certificate">Свидетельство о рождении</option>
                    <option value="driving-license">Вод. удостоверение</option>
                  </select>
                </div>
              </div>
            

            <div class="serial-number">
              <!-- Серия -->
              <div class="input-text">
                <label for="passport__serial">Серия</label>
                <input type="text" id="passport__serial" placeholder="Серия" />
              </div>

              <!-- Номер -->
              <div class="input-text">
                <label for="passport__number">Номер</label>
                <input type="text" id="passport__number" placeholder="Номер" />
              </div>
            </div>

            <!-- Кем выдан -->
            <label for="passport__whom">Кем выдан</label>
            <input type="text" id="passport__whom" placeholder="Кем выдан" />

            <!-- Дата выдачи -->
            <label for="passport__when" :class="$v.form.passportWhen.$error ? 'invalid-label' : ''">Дата выдачи*</label>
            <input type="date" id="passport__when" placeholder="Дата выдачи*" :class="$v.form.passportWhen.$error ? 'invalid-input' : ''"/>
          </div>
        </div>
        <button type="submit" class="c-button">Создать</button>
      </form>
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, numeric } from "vuelidate/lib/validators";
export default {
  mixins: [validationMixin],
  data() {
    return {
      isClientCreate: false,
      groupClients: {
        vip: "VIP",
        problem: "Проблемные",
        oms: "ОМС",
      },
      form: {
        checkedGroupClient: [],
        surname: "",
        name: "",
        middleName: "",
        phone: "",
        city: "",
        birthdate: "",
        gender: "Male",
        documentType: "passport",
        passportWhen: ""
      },
    };
  },
  validations: {
    form: {
      surname: {
        required,
      },
      name: {
        required,
      },
      birthdate: {
        required,
      },
      phone: {
        required,
        numeric,
      },
      checkedGroupClient: {
        required,
      },
      city: {
        required
      },
      documentType: {
        required
      },
      passportWhen: {
        required
      }
    },
  },
  methods: {
    createClient() {
      this.$v.form.$touch();
      if (this.$v.form.$error) {
        window.scrollTo(0, 0);
      } else {
        window.scrollTo(0, 0);
        this.isClientCreate = true;
      }
    },
  },
  
};
</script>

<style lang="scss">
@import "@/assets/main.scss";
@import "@/assets/variables.scss";

.wrapper {
  display: flex;
  flex-direction: column;
  padding: 50px;
@media screen and (max-width: 480px) {
    padding: 5px;
  }
}

.container {
  max-width: 600px;
  margin: 0px auto;
  width: 100%;
}

.create-client {
  border-radius: 5px;
  padding: 35px;
  box-shadow: 0 0 10px 5px rgba(221, 221, 221, 1);
  input[type="text"],
  input[type="date"] {
    width: 100%;
    margin-bottom: 15px;
    padding: 15px;
    border: none;
    background: #f1f1f1;
    font-size: 1em;
    &:focus {
      background-color: #ddd;
      outline: none;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }
    &[type="date"] {
      max-width: 250px;
    }
  }
}

.client-created {
  width: 100%;
  height: 50px;
  background-color: rgb(12, 179, 12);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 20px;
  border-radius: 3px;
  box-shadow: 0 0 10px 5px rgba(221, 221, 221, 1);
}

//Основная информация
.main-info {
  &__title {
    @include title;
  }
  &__row {
    @include row;
  }
  &__selectors-row {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-bottom: $mbf;
  }
  &__select {
    @include selector;
  }
  &__multi-select {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  &__checkbox {
    margin-bottom: $mbf;
    display: flex;
    width: 100%;
    justify-content: center;
    label {
      font-weight: 700;
    }
    input {
      width: 20%;
    }
  }
}

//Адрес
.adress {
  &__title {
    @include title;
  }
  &__row {
    display: flex;
    flex-wrap: wrap;
    margin-top: $mtf;
    justify-content: space-between;
    :nth-child(2n) {
      input {
        width: 100%;
      }
    }
    :nth-child(2n + 1) {
      input {
        width: 90%;
      }
    }
  }
  .input-text {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    width: 48%;
    @media screen and (max-width: 480px) {
      width: 100%;
    }
    label {
      margin-left: 10px;
      font-size: 0.9em;
    }
  }
}

// Документ
.passport {
  &__title {
    @include title;
  }
  &__row {
    @include row;
  }
  .serial-number {
    display: flex;
    justify-content: space-between;
    width: 100%;
    :nth-child(2) {
      input {
        width: 100%;
        
      }
    }
    :nth-child(1) {
      input {
        width: 90%;
        @media screen and (max-width: 480px) {
        width: 80%;
    }
      }
    }
  }
  .input-text {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    width: 48%;
    @media screen and (max-width: 480px) {
      width: 100%;
    }
    label {
      margin-left: 10px;
      font-size: 0.9em;
    }
  }
  &__select {
    @include selector;
  }
  &__selectors-row {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-bottom: $mbf;
  }
}

//Кнопка отправки формы
.c-button {
  @include btn-send;
}

//Валидация
.wrapper .container form .invalid-input {
  border: 1px solid $error;
}
.wrapper .container form .invalid-label {
  color: $error;
}
.error-input {
  color: $error;
  margin-left: 10px;
  margin-top: -13px;
  font-size: 10px;
  &__group-clients {
    color: $error;
    margin-left: 10px;
    margin-top: 0;
    font-size: 10px;
  }
}
</style>
