<template>
  <button class="operationbutton">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "CalculatorButton",
  data() {
    return {};
  }
};
</script>

<style>
.operationbutton {
  height: 29px;
  width: 35px;
  margin-left: 3px;
  margin-top: 3px;
}

.backButton {
  height: 29px;
  width: 78px;
  margin-left: 3px;
  margin-top: 3px;
}

.equal {
  /* position:absolute; */
  position: absolute;
  height: 61px;
  width: 35px;
  margin-left: 8px;
}

.zero {
  height: 29px;
  width: 78px;
}
</style>