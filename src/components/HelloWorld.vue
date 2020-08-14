<template>
  <div class="wrapper">
    <div class="container">
      <form class="create-client" @submit.prevent="createClient">
        <div class="main-info">
          <span class="main-info__title">Основная информация</span>
          <div class="main-info__row">
            <label for="surname" :class="$v.form.surname.$error ? 'invalidLabel' : ''">Фамилия*</label>
            <input type="text" :class="$v.form.surname.$error ? 'invalidInput' : ''" id="surname" placeholder="Фамилия*" v-model.trim="form.surname" />
            <p v-if="$v.form.surname.$dirty && $v.form.surname.required">Обязательное поле</p>
            <label for="name">Имя*</label>
            <input type="text" id="name" placeholder="Имя*" v-model.trim="form.name" />
            <!-- <label for="middle-name">Отчество</label>
            <input type="text" id="middle-name" placeholder="Отчество" />-->

            <label for="birthdate" min="1920-01-01" max="2020-01-01">Дата рождения*</label>
            <input type="date" name="birthdate" id="birthdate" v-model="form.birthdate"/>

            <label for="phone-text">Номер телефона*</label>
            <input
              type="text"
              id="phone-text"
              pattern="[(][0-9]{3}[)] [0-9]{3}-[0-9]{4}"
              placeholder="+7 (___)___-__-__"
              v-model.trim="form.phone"
            />

            <div class="main-info__selectors-row">
              <div class="main-info__select">
                <label for="gender">Пол</label>
                <select name="gender" id="gender">
                  <option value="Male">Мужской</option>
                  <option value="Female">Женский</option>
                </select>
              </div>

              <div class="main-info__multi-select">
                <label>Группа клиентов*</label>
                <div>
                <input type="checkbox" id="vip" value="vip" v-model="form.checkedGroupClient">
                <label for="vip">VIP</label>
                </div>
                <div>
                <input type="checkbox" id="problem" value="problem" v-model="form.checkedGroupClient">
                <label for="poblem">Проблемные</label>
                </div>
                <div>
                <input type="checkbox" id="oms" value="oms" v-model="form.checkedGroupClient">
                <label for="oms">ОМС</label>
                </div>
              </div>

              <div class="main-info__select">
                <label for="treating-doctor">Лечащий врач</label>
                <select name="treating-doctor" id="treating-doctor">
                  <option>Иванов</option>
                  <option>Захаров</option>
                  <option>Чернышева</option>
                </select>
              </div>
            </div>

            <div class="main-info__checkbox">
              <label for="not-send-sms">Не отправлять СМС</label>
              <input type="checkbox" name="not-send-sms" id="not-send-sms" />
            </div>
          </div>
        </div>
        <div class="adress">
          <span class="adress__title">Адрес</span>
          <div class="adress__row">
            <!-- <div class="input-text">
              <label for="index">Индекс</label>
              <input type="text" id="index" placeholder="Индекс" />
            </div>
            <div class="input-text">
              <label for="country">Страна</label>
              <input type="text" id="country" placeholder="Страна" />
            </div>-->
            <!-- <div class="input-text">
              <label for="region">Область</label>
              <input type="text" id="region" placeholder="Область" />
            </div>-->
            <div class="input-text">
              <label for="city">Город*</label>
              <input type="text" id="city" placeholder="Город*" v-model.trim="form.city" />
            </div>
            <!-- <div class="input-text">
              <label for="street">Улица</label>
              <input type="text" id="street" placeholder="Улица" />
            </div>
            <div class="input-text">
              <label for="house">Дом</label>
              <input type="text" id="house" placeholder="Дом" />
            </div>-->
          </div>
        </div>
        <div class="passport">
          <span class="passport__title">Паспорт</span>
          <div class="passport__row">
            <label for="document-type">Тип документа*</label>

            <div>
              <div class="passport__selectors-row">
                <div class="passport__select">
                  <select name="document-type" id="document-type">
                    <option value="passport">Паспорт</option>
                    <option value="birth-certificate">Свидетельство о рождении</option>
                    <option value="driving-license">Вод. удостоверение</option>
                  </select>
                </div>
              </div>
            </div>

            <!-- <label for="passport__serial">Серия</label>
                <input type="text" id="passport__serial" placeholder="Серия" />
  
                <label for="passport__number">Номер</label>
                <input type="text" id="passport__number" placeholder="Номер" />
             


            <label for="passport__whom">Кем выдан</label>
            <input type="text" id="passport__whom" placeholder="Кем выдан" />-->

            <label for>Дата выдачи*</label>
            <input type="date" id="passport__when" placeholder="Дата выдачи*" />
          </div>
        </div>
        <button type="submit">Создать</button>
      </form>
    </div>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate';
import { required, minLength} from 'vuelidate/lib/validators'
export default {
  mixins: [validationMixin],
  data() {
    return {
      form: {
      checkedGroupClient: [],
      surname: "",
      name: "",
      phone: "",
      city: "",
      birthdate: ""
      }
    };
  },
  validations: {
    form: {
    surname: {
     required,
     minLength: minLength(5)
    }
    }
  },
  methods: {
    createClient() {
      this.$v.form.$touch()
      if (this.$v.form.$error) {
        console.log('ok')
      }
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/main.scss";
@import "@/assets/variables.scss";
.wrapper .container form .invalidInput {
  border: 1px solid red;
}
.wrapper .container form .invalidLabel {
  color: red;
}

.wrapper {
  display: flex;
  flex-direction: column;
  padding: 50px;
}

.container {
  max-width: 600px;
  margin: 0px auto;
  width: 100%;
}

.create-client {
  border-radius: 5px;
  padding: 50px;
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
.adress {
  &__title {
    @include title;
  }
  &__row {
    display: flex;
    flex-wrap: wrap;
    margin-top: $mtf;
  }
  .input-text {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    width: 48%;
    margin-left: 5px;
    @media screen and (max-width: 480px) {
      width: 100%;
    }
    label {
      margin-left: 10px;
      font-size: 0.8em;
    }
  }
}

.passport {
  &__title {
    @include title;
  }
  &__row {
    @include row;
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
</style>
