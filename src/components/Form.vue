<template lang="pug">
  form(class="form")
    p(class="form-description") Чтобы рассчитать стоимость доставки, нужно ответить всего на 3 вопроса.
    Button
    List
</template>

<script>
import Button from '@/components/Button'
import List from '@/components/List'

export default {
  name: 'Calculator',
  components: {
    Button,
    List
  }
}
</script>

<style lang="scss">

.form-description {
  margin-top: 60px;
  margin-bottom: 62px;
  font-family: "Sero Pro Medium";
  font-size: 20px;
}

.form {
  box-sizing: border-box;
  width: 1024px;
  background: #f9f9f3;
  margin: 0 auto;
  padding-left: 100px;
  padding-right: 40px;
  padding-top: 40px;
  display: flex;
  flex-wrap: wrap;
}
</style>
