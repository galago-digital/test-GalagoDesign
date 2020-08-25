<template lang="pug">
  ul(class="list")
    li
      fieldset(class="fieldset")
        h2(class="fieldset__question") 1. Какой вес у вашей посылки?
        ul(class="fieldset__list")
          li
            input(type="radio" class="fieldset__radio" name="weight-parcel" id="up-to-5")
            label(for="up-to-5" class="fieldset__label") До 5 кг
          li
            input(type="radio" class="fieldset__radio" name="weight-parcel" id="from-5-to-10")
            label(for="from-5-to-10" class="fieldset__label") От 5 до 10 кг
          li
            input(type="radio" class="fieldset__radio" name="weight-parcel" id="over-10")
            label(for="over-10" class="fieldset__label") Больше 10 кг
    li
      fieldset(class="fieldset")
        h2(class="fieldset__question") 2. Оформим курьерскую доставку до двери получателя?
        ul(class="fieldset__list")
          li
            input(type="radio" class="fieldset__radio" name="delivery" id="yes")
            label(for="yes" class="fieldset__label") Да
          li
            input(type="radio" class="fieldset__radio" name="delivery" id="no")
            label(for="no" class="fieldset__label") Нет
    li
      fieldset(class="fieldset")
        h2(class="fieldset__question") 3. Хотите застраховать посылку?
        ul(class="fieldset__list")
          li
            input(type="radio" class="fieldset__radio" name="insurance" id="yes-insurance")
            label(for="yes-insurance" class="fieldset__label") Да, на сумму
          li
            input(type="radio" class="fieldset__radio" name="insurance" id="no-insurance")
            label(for="no-insurance" class="fieldset__label") Нет
</template>

<style lang="scss">
  .list {
    width: 900px;
    list-style: none;
    border: none;
    margin: 0;
    padding: 0;
  }

  .fieldset {
    border: none;
    padding: 0;
    margin: 0;
  }

  .fieldset__question {
    font-family: "Sero Pro Medium";
    font-weight: 500;
    font-size: 18px;
    line-height: 28px;
    text-align: left;
    margin: 0;
    margin-bottom: 22px;
  }

  .fieldset__list {
    list-style: none;
    border: none;
    margin: 0;
    padding: 0;
    display: flex;
    margin-bottom: 72px;
  }

  .fieldset__label {
    font-family: "Sero Pro";
    font-weight: 400;
    font-size: 18px;
  }
</style>
