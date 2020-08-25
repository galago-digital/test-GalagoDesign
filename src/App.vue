<template  lang="pug">
  div(id="app")
    Form
</template>

<script>
import Form from '@/components/Form'

export default {
  name: 'App',
  components: {
    Form
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.button {
  align-self: flex-start;
  padding: 0;
  margin-left: auto;
  border: none;
  background-color: transparent;
}
</style>
