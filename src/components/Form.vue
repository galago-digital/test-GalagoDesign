<template lang="pug">
  form(class="form")
    button(class="button")
    p(class="form-description") Чтобы рассчитать стоимость доставки, нужно ответить всего на 3 вопроса.
    ul(class="list")
      li
        fieldset(class="fieldset")
          h2(class="fieldset__question") 1. Какой вес у вашей посылки?
          ul(class="fieldset__list")
            li
              input(
              type="radio"
              class="fieldset__radio visually-hidden"
              name="weight-parcel"
              id="up-to-5"
              value="99"
              v-model="weight"
              @change="onOverweightChecked")
              label(for="up-to-5" class="fieldset__label") До 5 кг
            li
              input(
              type="radio"
              class="fieldset__radio visually-hidden"
              name="weight-parcel"
              id="from-5-to-10"
              value="199"
              v-model="weight"
              @change="onOverweightChecked")
              label(for="from-5-to-10" class="fieldset__label") От 5 до 10 кг
              div(v-if="this.isCanBeBrokenChecked")
                input(type="number" class="fieldset__extra-input" id="fieldset-extra-input" v-model="parts" min="2")
                label(for="fieldset-extra-input" class="fieldset__extra-label") части
            li
              input(
                type="radio"
                class="fieldset__radio visually-hidden"
                name="weight-parcel" id="over-10"
                value="Больше 10 кг"
                v-model="weight"
                @change="onOverweightChecked"
                checked=isOverweightChecked
                :disabled="isDisabled")
              label(for="over-10" class="fieldset__label fieldset__overweight") Больше 10 кг
              div(class="fieldset__extra" v-if="this.isOverweightChecked")
                p(class="fieldset__extra-description") <span class="fieldset__extra-description--orange">Мы не отправляем посылки более 10 кг.</span> Её можно разбить на части до 10 кг?
                ul(class="fieldset__extra-list")
                  li
                    input(type="radio"
                    class="fieldset__radio visually-hidden"
                    name="overweight"
                    id="over-10-yes"
                    value="over-10-yes"
                    v-model="overweight"
                    @change="onCanBeBrokenChecked")
                    label(for="over-10-yes" class="fieldset__label fieldset__label--extra") Да
                  li
                    input(type="radio"
                    class="fieldset__radio visually-hidden"
                    name="overweight"
                    id="over-10-no"
                    value="over-10-no"
                    v-model="overweight"
                    @change="onCanBeBrokenChecked")
                    label(for="over-10-no" class="fieldset__label fieldset__label--extra") Нет
      li
        fieldset(class="fieldset")
          h2(class="fieldset__question") 2. Оформим курьерскую доставку до двери получателя?
          ul(class="fieldset__list")
            li
              input(type="radio"
              class="fieldset__radio visually-hidden"
              name="delivery"
              id="yes-delivery"
              value="50"
              v-model="delivery"
              )
              label(for="yes-delivery" class="fieldset__label") Да
            li
              input(type="radio"
              class="fieldset__radio visually-hidden"
              name="delivery"
              id="no-delivery"
              value="0"
              v-model="delivery"
              )
              label(for="no-delivery" class="fieldset__label") Нет
      li
        fieldset(class="fieldset")
          h2(class="fieldset__question") 3. Хотите застраховать посылку?
          ul(class="fieldset__list")
            li
              input(type="radio"
              class="fieldset__radio visually-hidden"
              name="insurance"
              id="yes-insurance"
              v-model="insurance"
              value="insurance-yes"
              @change="onInsuranceChecked"
              :disabled="isInsuranceCheckboxDisabled")
              label(for="yes-insurance" class="fieldset__label") Да, на сумму
              div(class="input__insurance-wrap" v-bind:class="{'input__insurance-orange': this.insuranceSum > maxInsuranceSum}")
                input(type="number"
                class="input__insurance"
                v-model="insuranceSum"
                :disabled="isInsuranceDisabled"
                v-bind:class="{'warn-color': this.insuranceSum > maxInsuranceSum}")
                p(class="warn-message" v-bind:class="{'show-warn-message': this.insuranceSum > maxInsuranceSum}") Максимальная объявленная стоимость посылок в Сберлогистике 150 000 ₽
            li
              input(type="radio"
              class="fieldset__radio visually-hidden"
              name="insurance"
              id="no-insurance"
              value="insurance-no"
              v-model="insurance"
              @change="onInsuranceChecked"
              :disabled="isInsuranceCheckboxDisabled")
              label(for="no-insurance" class="fieldset__label") Нет
    div(class="result-wrap" v-bind:class="{warn: this.overweight === this.InputValues.OVER_10_NO}")
      p(class="result-wrap_message"
       v-bind:class="{'warn-text': this.overweight === this.InputValues.OVER_10_NO, 'warn-limit': this.insuranceSum > maxInsuranceSum}") {{ resumeMessage ? resumeMessage : 'Отправка будет стоить всего'}}
      p(class="result-wrap_cost" v-bind:class="{'visually-hidden': this.overweight === this.InputValues.OVER_10_NO}") {{ calcPayment }}
</template>

<script>
export default {
  name: 'Form',
  components: {
  },
  data () {
    return {
      weight: '',
      delivery: '',
      overweight: '',
      isOverweightChecked: '',
      isCanBeBrokenChecked: '',
      parts: '',
      insuranceSum: '',
      insurance: '',
      isDisabled: false,
      isInsuranceDisabled: true,
      isInsuranceCheckboxDisabled: false,
      isDeliveryCheckboxDisabled: false,
      resumeMessage: '',
      percentFromInsurance: 0.002,
      priceForDelivery: 50,
      priceFor10kg: 199,
      maxInsuranceSum: 150000,
      InputValues: {
        OVER_10_YES: 'over-10-yes',
        OVER_10_NO: 'over-10-no',
        DELIVERY_YES: '50',
        INSURANCE_YES: 'insurance-yes',
        INSURANCE_NO: 'insurance-no',
        OVERWEIGHT: 'Больше 10 кг',
        UP_TO_10_KG: '199'
      },
      Messages: {
        SUCCESSFUL: 'Отправка будет стоить всего',
        NOT_SUCCESSFUL: 'К сожалению, мы не сможем отправить вашу посылку через Сберлогистику.',
        OVER_INSURANCE: 'Отправка застрахованной посылки с объявленной стоимостью 150 000 ₽ будет стоить всего'
      }
    }
  },
  computed: {
    calcPayment () {
      let payment = ((this.weight * 1) || 0) + (this.delivery * 1) || 0
      if (this.overweight === this.InputValues.OVER_10_YES) {
        payment -= this.priceFor10kg
        if (this.delivery === this.InputValues.DELIVERY_YES) {
          payment -= this.priceForDelivery
        }
        payment += (this.parts * this.priceFor10kg) + (this.delivery * this.parts)
      }
      if (this.insurance === this.InputValues.INSURANCE_YES && this.insuranceSum > this.maxInsuranceSum) {
        payment += (this.maxInsuranceSum * this.percentFromInsurance)
      } else if (this.insurance === this.InputValues.INSURANCE_YES && this.insuranceSum <= this.maxInsuranceSum) {
        payment += (this.insuranceSum * this.percentFromInsurance)
      }
      return Math.round(parseFloat(payment) * 100) / 100
    }
  },
  methods: {
    onOverweightChecked () {
      if (this.weight === this.InputValues.OVERWEIGHT) {
        this.isOverweightChecked = true
      } else if (this.insuranceSum > this.maxInsuranceSum) {
        this.isOverweightChecked = false
        this.isCanBeBrokenChecked = false
        this.overweight = ''
        this.isDisabled = false
        this.resumeMessage = this.Messages.OVER_INSURANCE
      } else {
        this.isOverweightChecked = false
        this.isCanBeBrokenChecked = false
        this.overweight = ''
        this.isDisabled = false
        this.resumeMessage = this.Messages.SUCCESSFUL
        this.isInsuranceCheckboxDisabled = false
      }
    },
    onCanBeBrokenChecked () {
      if (this.overweight === this.InputValues.OVER_10_YES && this.insuranceSum > this.maxInsuranceSum) {
        this.isInsuranceCheckboxDisabled = false
        this.isCanBeBrokenChecked = true
        this.weight = this.InputValues.UP_TO_10_KG
        this.isDisabled = true
        this.resumeMessage = this.Messages.OVER_INSURANCE
      } else if (this.overweight === this.InputValues.OVER_10_YES) {
        this.isInsuranceCheckboxDisabled = false
        this.isCanBeBrokenChecked = true
        this.weight = this.InputValues.UP_TO_10_KG
        this.isDisabled = true
        this.resumeMessage = this.Messages.SUCCESSFUL
      } else if (this.overweight === this.InputValues.OVER_10_NO) {
        this.insurance = ''
        this.isInsuranceDisabled = true
        this.insuranceSum = ''
        this.isInsuranceCheckboxDisabled = true
        this.isCanBeBrokenChecked = false
        this.isDisabled = true
        this.weight = this.InputValues.OVERWEIGHT
        this.resumeMessage = this.Messages.NOT_SUCCESSFUL
      }
    },
    onInsuranceChecked () {
      if (this.insurance === this.InputValues.INSURANCE_YES) {
        this.isInsuranceDisabled = false
      }
      if (this.insurance === this.InputValues.INSURANCE_NO) {
        this.insurance = ''
        this.isInsuranceDisabled = true
        this.insuranceSum = ''
        this.resumeMessage = this.Messages.SUCCESSFUL
      }
    }
  },
  watch: {
    insuranceSum: function () {
      if (this.insuranceSum > this.maxInsuranceSum) {
        this.resumeMessage = this.Messages.OVER_INSURANCE
      } else if (this.insuranceSum <= this.maxInsuranceSum && this.overweight !== this.InputValues.OVER_10_NO) {
        this.resumeMessage = this.Messages.SUCCESSFUL
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.form {
  width: 64em;
  background: #f9f9f3;
  margin: 0 auto;
  padding-top: 2.7em;
  display: flex;
  flex-wrap: wrap;
  position: relative;
}
.form-description {
  @include font("Sero Pro Medium", 1.25em);
  margin-top: 2.8em;
  margin-bottom: 3.1em;
  margin-left: 5.1em;
}

.button {
  padding: 0;
  border: none;
  background-color: transparent;
  position: absolute;
  right: 3.1em;
  background-image: url("../assets/img/button.svg");
  width: 3em;
  height: 3em;
}

.list {
  @include list;
  width: 56.25em;
  border: none;
  margin-left: 6.4em;
    li {
      &:last-child {
        .fieldset__list {
          margin-bottom: 0;
        }
      }
    }
}

.fieldset {
  border: none;
  padding: 0;
  margin: 0;
}

.fieldset__question {
  @include font("Sero Pro Medium", 1.125em, 500);
  line-height: 1.6em;
  text-align: left;
  margin: 0;
  margin-bottom: 1.13em;
}

.fieldset__list {
  @include list;
  border: none;
  display: flex;
  margin-bottom: 4.43em;
    li {
      margin-right: 9.68em;
      width: 11.21em;
      position: relative;
        &:nth-child(3n) {
          margin-right: 0;
      }
    }
}

.fieldset__label {
  @include font("Sero Pro Medium", 1.125em);
  padding-left: 2.8em;
  padding-top: 1em;
  padding-bottom: 1em;

    &::before {
      content: "";
      position: absolute;
      width: 2.125em;
      height: 2.125em;
      background-image: url("../assets/img/ellipse1.svg");
      background-repeat: no-repeat;
      top: -0.4em;
      left: 0;
    }
}

.fieldset__radio:checked~.fieldset__label::after {
  content: "";
  position: absolute;
  width: 2.125em;
  height: 2.125em;
  background-image: url("../assets/img/ellipse2.svg");
  background-repeat: no-repeat;
  left: 0.35em;
  top: -0.05em;
}

.input__insurance::-webkit-outer-spin-button,
.input__insurance::-webkit-inner-spin-button,
.fieldset__extra-input::-webkit-outer-spin-button,
.fieldset__extra-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.input__insurance[type=number],
.fieldset__extra-input[type=number] {
    -moz-appearance:textfield;
}

.input__insurance {
  @include font("Sero Pro", 1.875em, 500, $bgcolor-green);
  background-color: transparent;
  margin-top: 0.16em;
  margin-left: 1.67em;
  padding-left: 0.35em;
  padding-top: 0.2em;
  width: 3.922em;
  height: 1.5em;
  border-radius: 0.2em;
  border-color: #bcbcb7;
  border-style: solid;
}

.input__insurance-wrap {
  width: 13em;
  height: 6.25em;
  background-image: url("../assets/img/ruble_sign.svg");
  background-repeat: no-repeat;
  background-position: 11.1em 1.1em;
}

.input__insurance-orange {
  background-image: url("../assets/img/ruble_sign_orange.svg");
}

.fieldset__extra {
  width: 14.375em;
  height: 22.4em;
  border-color: #bcbcb7;
  border-style: solid;
  border-width: 0.01em;
  border-radius: 1.3em;
  position: absolute;
  top: -1.61em;
  left: -1.2em;
  background-color: #fff;
}

.fieldset__extra-description {
  @include font("Sero Pro Medium", 1.125em);
  width: 9.9em;
  line-height: 1.47em;
  margin: 0;
  margin-top: 6.6em;
  margin-left: 1.1em;
}

.fieldset__extra-description--orange {
  color: #ff7f00;
}

.fieldset__extra-list {
  list-style: none;
  border: none;
  margin: 0;
  padding: 0;

    li {
      margin-top: 1.938em;
      padding-left: 3.688em;
    }
}

.fieldset__overweight {
  position: relative;
  z-index: 1;
    &::before {
      top: 0.59em;
      left: 0.1em;
    }
}

.fieldset__radio:checked~.fieldset__overweight::after {
  top: 0.93em;
  left: 0.4em;
}

.fieldset__label--extra {
  padding-left: 0.6em;
    &::before {
      left: 1.09em;
      top: -0.3em;
      padding-left: 11em;
      padding-top: 2.5em;
    }
}

.fieldset__radio:checked~.fieldset__label--extra::after {
  left: 1.46em;
  top: 0;
}

.fieldset__extra-input {
  @include font("Sero Pro", 1.875em, 400, $bgcolor-green);
  background-color: transparent;
  margin-top: 0.19em;
  margin-left: 1.7em;
  padding-left: 0.35em;
  padding-top: 0.2em;
  width: 1.5em;
  height: 1.5em;
  border-radius: 0.2em;
  border-color: #bcbcb7;
  border-style: solid;
  position: absolute;
}

.fieldset__extra-label {
  @include font("Sero Pro", 1.125em, 500);
  position: absolute;
  top: 2.21em;
  right: 1.6em;
}
.result-wrap {
  background-color: #FFF;
  width: 100%;
  display: flex;
  justify-content: flex-end;
  padding-top: 1.563em;
  position: relative;
}

.result-wrap_message {
  @include font("Sero Pro Medium", 1.250em, 500);
  line-height: 1.4em;
  width: 13em;
  text-align: end;
  margin: 0;
  margin-right: 0.445em;
}

.result-wrap_cost {
  @include font("Sero Pro", 3.5em, 500, $bgcolor-green);
  line-height: 0.5em;
  margin: 0;
  padding-right: 2.48em;
  padding-top: 0.3em;

    &::after {
      content: "";
      position: absolute;
      width: 1.938em;
      height: 2.5em;
      background-image: url("../assets/img/ruble_sign_big.svg");
      background-repeat: no-repeat;
      top: 0.6em;
      right: 0.4em;
    }
}

.warn {
  justify-content: center;
}

.warn-text {
  width: 37em;
  color: #ff7f00;
  margin-right: 5.6em;
  margin-top: 1em;
}

.warn-limit {
  width: 29em;
}

.warn-message {
  @include font("Sero Pro Medium", 1.125em, 500, #ff7f00);
  width: 43em;
  margin-left: 2.8em;
  margin-top: 0.6em;
  margin-bottom: 0;
  visibility: hidden;
}

.show-warn-message {
  visibility: visible;
}

.warn-color {
  color: #ff7f00;
}
</style>
