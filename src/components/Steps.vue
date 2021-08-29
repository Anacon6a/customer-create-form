<template>
  <div class="box-step">
    <span class="step" :class="setStepIndicator(0)"></span>
    <span class="step" :class="setStepIndicator(1)"></span>
    <span class="step" :class="setStepIndicator(2)"></span>
  </div>
</template>

<script>
export default {
  props: {
      currentTab: Number
  },
  methods: {
    setStepIndicator(stepNumber) {
      if (this.currentTab == stepNumber) {
        return "step_active";
      } else if (this.currentTab > stepNumber) {
        return "step_finish";
      } else {
        return;
      }
    },
  },
};
</script>

<style lang="sass">
$purple: #423189
.box-step
    text-align: center
    margin-top: 25px
@media (max-width: 480px)
    margin-top: 15px

.step
    height: 13px
    width: 13px
    margin: 0 2px
    background-color: #bbbbbb
    border: none
    border-radius: 50%
    display: inline-block
    opacity: 0.6
@media (max-width: 480px)
    height: 10px
    width: 10px

.step_active
    background-color: $purple

.step_finish
    background-color: $purple
    opacity: 1
</style>