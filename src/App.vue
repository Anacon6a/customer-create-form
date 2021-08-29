<template>
  <CustomerCreateForm />
</template>

<script>
import CustomerCreateForm from "./components/CustomerCreateForm.vue";

export default {
  name: "App",
  components: {
    CustomerCreateForm,
  },
};
</script>

<style lang="sass">
@font-face
  font-family: 'Montserrat'
  src: url('./assets/fonts/Montserrat-Medium.woff') format('woff')
  src: url('./assets/fonts/Montserrat-Medium.ttf') format('truetype')
  font-weight: medium
  font-style: normal
@font-face
  font-family: 'Montserrat'
  src: url('./assets/fonts/Montserrat-ExtraBold.woff') format('woff')
  src: url('./assets/fonts/Montserrat-ExtraBold.ttf') format('truetype')
  font-weight: bold
  font-style: normal
@font-face
  font-family: 'CourierNew'
  src: url('./assets/fonts/CourierNewPSMT.woff') format('woff')
  src: url('./assets/fonts/CourierNewPSMT.ttf') format('truetype')

html
  font-size: 62.5%

#app
  font-family: CourierNew, Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  margin: 30px 15px
  @media (max-width: 480px)
    margin: 10px 0px

body
  background: #eee
</style>