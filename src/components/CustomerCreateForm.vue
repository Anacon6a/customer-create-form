<template>
  <form class="form" @submit.prevent="goNext" novalidate>
    <h1 class="form__title">Добавление клиента</h1>

    <Steps :currentTab="currentTab" />

    <div v-if="currentTab == 0">
      <div class="form__group">
        <label class="form__label">Имя</label
        ><span class="symb-required">*</span>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[0].firstName"
          :class="
            v$.form.tabs[0].firstName.$dirty &&
            v$.form.tabs[0].firstName.$invalid
              ? 'input_error'
              : ''
          "
        />
        <span
          v-if="
            v$.form.tabs[0].firstName.$dirty &&
            v$.form.tabs[0].firstName.required.$invalid
          "
          class="text_error"
        >
          &#10006; Поле «Имя» не должно быть пустым
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Фамилия</label
        ><span class="symb-required">*</span>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[0].lastName"
          :class="
            v$.form.tabs[0].lastName.$dirty && v$.form.tabs[0].lastName.$invalid
              ? 'input_error'
              : ''
          "
        />
        <span
          v-if="
            v$.form.tabs[0].lastName.$dirty &&
            v$.form.tabs[0].lastName.required.$invalid
          "
          class="text_error"
        >
          &#10006; Поле «Фамилия» не должно быть пустым
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Отчество</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[0].middleName"
        />
      </div>

      <div class="row row_space-between">
        <div class="form__group row__group">
          <label class="form__label">Дата рождения</label
          ><span class="symb-required">*</span>
          <input
            type="date"
            v-model="form.tabs[0].birthDate"
            class="input form__input"
            :class="
              v$.form.tabs[0].birthDate.$dirty &&
              v$.form.tabs[0].birthDate.$invalid
                ? 'input_error'
                : ''
            "
          />
        </div>

        <div class="form__group row__group">
          <label class="form__label">Номер телефона</label
          ><span class="symb-required">*</span>
          <PhoneInput
            :phoneNumber="form.tabs[0].phoneNumber"
            @phoneInput="form.tabs[0].phoneNumber = $event"
            :class="
              v$.form.tabs[0].phoneNumber.$dirty &&
              v$.form.tabs[0].phoneNumber.$invalid
                ? 'input_error'
                : ''
            "
          />
        </div>
      </div>
      <div v-if="v$.form.tabs[0].birthDate.$dirty" class="text_error">
        <span v-if="v$.form.tabs[0].birthDate.required.$invalid">
          &#10006; Поле «Дата рождения» не должно быть пустым
        </span>
        <span v-if="v$.form.tabs[0].birthDate.maxValue.$invalid">
          &#10006; Дата не может быть больше текущей
        </span>
      </div>
      <div v-if="v$.form.tabs[0].phoneNumber.$dirty" class="text_error">
        <span v-if="v$.form.tabs[0].phoneNumber.required.$invalid">
          &#10006; Поле «Номер телефона» не должно быть пустым
        </span>
        <span
          v-if="
            v$.form.tabs[0].phoneNumber.minLength.$invalid ||
            v$.form.tabs[0].phoneNumber.numeric.$invalid
          "
        >
          &#10006; Номер телефона имеет неверный формат
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Пол</label>
        <div class="row">
          <div>
            <input
              type="radio"
              class="form__radio"
              name="gender"
              v-model="form.tabs[0].gender"
              value=""
              checked
            />
            <label class="form__label radio-label">Не выбран</label>
          </div>
          <div>
            <input
              type="radio"
              class="form__radio"
              name="gender"
              value="male"
              v-model="form.tabs[0].gender"
            />
            <label class="form__label radio-label">Мужской</label>
          </div>
          <div>
            <input
              type="radio"
              class="form__radio"
              name="gender"
              value="female"
              v-model="form.tabs[0].gender"
            />
            <label class="form__label radio-label">Женский</label>
          </div>
        </div>
      </div>

      <div class="row row_space-between">
        <div class="form__group row__group">
          <label class="form__label">Группа клиентов</label
          ><span class="symb-required">*</span>
          <select
            v-model="form.tabs[0].customerGroup"
            class="form__select"
            :class="
              v$.form.tabs[0].customerGroup.$dirty &&
              v$.form.tabs[0].customerGroup.$invalid
                ? 'input_error'
                : ''
            "
          >
            <option selected value>Выберите группу</option>
            <option
              v-for="(customerGroup, index) in customerGroups"
              :key="index"
            >
              {{ customerGroup }}
            </option>
          </select>
        </div>

        <div class="form__group row__group">
          <label class="form__label">Лечайщий врач</label>
          <select class="form__select" v-model="form.tabs[0].doctor">
            <option selected value>Выберите врача</option>
            <option v-for="(doctor, index) in doctors" :key="index">
              {{ doctor }}
            </option>
          </select>
        </div>
      </div>
      <span
        v-if="
          v$.form.tabs[0].customerGroup.$dirty &&
          v$.form.tabs[0].customerGroup.required.$invalid
        "
        class="text_error"
      >
        &#10006; Необходимо выбрать один из предложенных вариантов для поля
        «Группа клиентов»
      </span>

      <div>
        <input
          type="checkbox"
          class="form__checkbox"
          v-model="form.tabs[0].disablingMessage"
        />
        <label class="form__label checkbox-label">Не отправлять СМС</label>
      </div>
    </div>

    <div v-if="currentTab == 1">
      <div class="form__group">
        <label class="form__label">Индекс</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[1].postcode"
          maxlength="9"
          :class="
            v$.form.tabs[1].postcode.$dirty && v$.form.tabs[1].postcode.$invalid
              ? 'input_error'
              : ''
          "
        />
        <span
          v-if="
            v$.form.tabs[1].postcode.$dirty &&
            v$.form.tabs[1].postcode.numeric.$invalid
          "
          class="text_error"
        >
          &#10006; Индекс имеет неверный формат
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Страна</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[1].country"
        />
      </div>

      <div class="form__group">
        <label class="form__label">Область</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[1].region"
        />
      </div>

      <div class="form__group">
        <label class="form__label">Город</label
        ><span class="symb-required">*</span>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[1].city"
          :class="
            v$.form.tabs[1].city.$dirty && v$.form.tabs[1].city.$invalid
              ? 'input_error'
              : ''
          "
        />
        <span
          v-if="
            v$.form.tabs[1].city.$dirty &&
            v$.form.tabs[1].city.required.$invalid
          "
          class="text_error"
        >
          &#10006; Поле «Город» не должно быть пустым
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Улица</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[1].street"
        />
      </div>

      <div class="form__group">
        <label class="form__label">Дом</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[1].house"
        />
      </div>
    </div>

    <div v-if="currentTab == 2">
      <div class="form__group">
        <label class="form__label">Тип документа</label
        ><span class="symb-required">*</span>
        <select
          v-model="form.tabs[2].documentType"
          class="form__select"
          :class="
            v$.form.tabs[2].documentType.$dirty &&
            v$.form.tabs[2].documentType.$invalid
              ? 'input_error'
              : ''
          "
        >
          <option selected value>Выберите документ</option>
          <option v-for="(document, index) in documents" :key="index">
            {{ document }}
          </option>
        </select>
        <span
          v-if="
            v$.form.tabs[2].documentType.$dirty &&
            v$.form.tabs[2].documentType.required.$invalid
          "
          class="text_error"
        >
          &#10006; Необходимо выбрать один из предложенных вариантов для поля
          «Тип документа»
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Серия</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[2].series"
          :class="
            v$.form.tabs[2].series.$dirty && v$.form.tabs[2].series.$invalid
              ? 'input_error'
              : ''
          "
        />
        <span
          v-if="
            v$.form.tabs[2].series.$dirty &&
            v$.form.tabs[2].series.numeric.$invalid
          "
          class="text_error"
        >
          &#10006; Серия имеет неправильный формат
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Номер</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[2].documentNumber"
          :class="
            v$.form.tabs[2].documentNumber.$dirty &&
            v$.form.tabs[2].documentNumber.$invalid
              ? 'input_error'
              : ''
          "
        />
        <span
          v-if="
            v$.form.tabs[2].documentNumber.$dirty &&
            v$.form.tabs[2].documentNumber.numeric.$invalid
          "
          class="text_error"
        >
          &#10006; Номер имеет неправильный формат
        </span>
      </div>

      <div class="form__group">
        <label class="form__label">Кем выдан</label>
        <input
          type="text"
          class="input form__input"
          v-model.trim="form.tabs[2].issuingAuthority"
        />
      </div>

      <div class="form__group">
        <label class="form__label">Дата выдачи</label
        ><span class="symb-required">*</span>
        <input
          type="date"
          class="input form__input"
          v-model="form.tabs[2].issueDate"
          :class="
            v$.form.tabs[2].issueDate.$dirty &&
            v$.form.tabs[2].issueDate.$invalid
              ? 'input_error'
              : ''
          "
        />
        <div v-if="v$.form.tabs[2].issueDate.$dirty" class="text_error">
          <span v-if="v$.form.tabs[2].issueDate.required.$invalid">
            &#10006; Поле «Дата выдачи» не должно быть пустым
          </span>
          <span v-if="v$.form.tabs[2].issueDate.maxValue.$invalid">
            &#10006; Дата не может быть больше текущей
          </span>
        </div>
      </div>
    </div>
    <div class="box-button">
      <button
        class="form__button"
        :class="currentTab == 0 ? 'button_hidden' : ''"
        @click="goBack"
        type="button"
      >
        Вернуться
      </button>
      <button class="form__button" type="submit">Далее</button>
    </div>
  </form>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { minLength, required, numeric } from "@vuelidate/validators";
import PhoneInput from "./PhoneInput.vue";
import Steps from "./Steps.vue";
export default {
  setup() {
    return { v$: useVuelidate() };
  },
  components: {
    PhoneInput,
    Steps,
  },
  data() {
    return {
      form: {
        tabs: [
          {
            firstName: "",
            lastName: "",
            middleName: "",
            birthDate: "",
            phoneNumber: "",
            gender: "",
            customerGroup: "",
            doctor: "",
            disablingMessage: false,
          },
          {
            city: "",
            postcode: "",
            country: "",
            region: "",
            street: "",
            house: "",
          },
          {
            documentType: "",
            series: "",
            documentNumber: "",
            issuingAuthority: "",
            issueDate: "",
          },
        ],
      },
      customerGroups: ["VIP", "Проблемные", "ОМС"],
      doctors: ["Иванов", "Захаров", "Чернышева"],
      documents: ["Паспорт", "Свидетельство о рождении", "Вод. удостоверение"],
      currentTab: 0,
    };
  },

  validations() {
    return {
      form: {
        $autoDirty: true,
        tabs: [
          {
            firstName: {
              required,
            },
            lastName: {
              required,
            },
            birthDate: {
              required,
              maxValue: (value) => value < new Date().toISOString(),
            },
            phoneNumber: {
              required,
              minLength: minLength(10),
              numeric,
            },
            customerGroup: {
              required,
            },
          },

          {
            postcode: {
              numeric,
            },
            city: {
              required,
            },
          },

          {
            documentType: {
              required,
            },
            series: {
              numeric,
            },
            documentNumber: {
              numeric,
            },
            issueDate: {
              required,
              maxValue: (value) => value < new Date().toISOString(),
            },
          },
        ],
      },
    };
  },
  methods: {
    goNext() {
      this.v$.form.tabs[this.currentTab].$touch();

      if (this.v$.form.tabs[this.currentTab].$error) return;

      switch (this.currentTab) {
        case 0:
        case 1:
          this.currentTab++;
          break;
        case 2:
          this.submit();
          break;
      }
    },
    goBack() {
      if (this.currentTab > 0) this.currentTab--;
    },
    submit() {
      alert("Клиент успешно создан!");
      this.resetForm();
      this.currentTab = 0;
    },
    resetForm() {
      for (let tab of this.form.tabs) {
        Object.keys(tab).forEach(function (key) {
          tab[key] = "";
        });
      }
      this.v$.$reset();
    },
  },
};
</script>

<style lang="sass">
$purple: #423189

.form
  background: white
  text-align: left
  border-radius: 20px
  padding: 15px 30px
  max-width: 450px
  margin: 0px auto
  font-size: 1.5rem

.form__title
  color: $purple
  font-family: 'Montserrat'
  font-size: 2.4rem
  @media (max-width: 480px)
    font-size: 2.0rem
  @media (max-width: 350px)
    font-size: 1.7rem

.form__label
  color: #aaa
  display: inline-block
  font-family: 'Montserrat'
  margin: 15px 0 5px
  font-size: 1.4rem
  @media (max-width: 480px)
    margin-top: 10px
  @media (max-width: 350px)
    font-size: 1.2rem

.radio-label
  margin-right: 15px
  color: #000
  font-family: CourierNew
  font-size: 1.5rem

.checkbox-label
  margin-bottom: 0
  font-size: 1.5rem
  color: #000
  font-family: CourierNew
  margin-top: 20px

.form__input, .form__select
  display: -webkit-inline-box
  -webkit-box-pack: center
  -webkit-box-align: center
  display: block
  padding: 10px 15px
  width: 100%
  box-sizing: border-box
  border: none
  font-family: CourierNew
  border-bottom: 1px solid #ddd
  font-size: 1.5rem
  color: #000
  outline-color: $purple
  height: 38px

.form__radio, .form__checkbox
  display: inline-block
  margin-right: 10px

.box-button
  display: flex
  justify-content: space-between
  flex-wrap: wrap

.form__button
  background: $purple
  border: 0
  padding: 10px 26px
  margin-top: 15px
  color: white
  border-radius: 20px
  font-family: CourierNew
  font-size: 1.5rem
  font-weight: 900
  @media (max-width: 350px)
    padding: 10px 20px
    font-size: 1.3rem

.form__button:hover
  @media (hover: hover)
    cursor: pointer
    opacity: 0.8

.button_hidden
  visibility: hidden

.row
  display: flex
  flex-wrap: wrap
  align-items: baseline

.row_space-between
  justify-content: space-between
  > .row__group
    width: calc(1/2*100% - (1 - 1/2)*20px)
    @media (max-width: 480px)
      width: 100%

.input_error
  border-color: red
  outline-color: red

.text_error
  color: red
  font-size: 1.4rem
  margin: 0 0 5px
  @media (max-width: 350px)
    font-size: 1.2rem

.symb-required
  color: red
</style>