<template>
  <div class="box-phone" :class="phoneFocus ? 'box-phone_focus' : ''">
    <span class="span-phone">+7</span>
    <input
      type="tel"
      class="phone-input"
      maxlength="10"
      @input="phoneInput"
      @focus="phoneFocus = true"
      @blur="phoneFocus = false"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      phoneFocus: false,
    };
  },
  props: {
    phoneNumber: String,
  },

  methods: {
    phoneInput(val) {
      this.$emit("phoneInput", val.target.value);
    },
  },
};
</script>

<style lang="sass">
$purple: #423189

.box-phone
  display: -webkit-inline-box
  -webkit-box-pack: center
  -webkit-box-align: center
  display: flex
  align-items: baseline
  border-bottom: 1px solid #ddd
  height: 38px
  box-sizing: border-box


.box-phone_focus
  border: 1px solid $purple
  box-sizing: border-box
  border-radius: 2px

.span-phone
  margin-left: 15px

.phone-input
  padding: 10px 15px 10px 0
  width: 100%
  box-sizing: border-box
  border: none
  font-family: CourierNew
  font-size: 1.5rem
  color: #000
  height: 38px
  outline: none
  background: transparent
</style>