<template>
  <div class="calculatorresult">
    <input type="text" class="inputup" v-model="processValue" readonly="readonly" />
    <input type="text" class="inputdown" v-model="inputvalue" readonly="readonly" />
  </div>
</template>

<script>
export default {
  name: "CalculatorResult",
  props: ["resultshow", "calprocess"],
  data() {
    return {};
  },

  computed: {
    inputvalue: function() {
      //   console.log(this.resultshow.value);
      return this.resultshow.value;
    },
    processValue: function() {
      return this.calprocess.value;
    }
  },

  watch: {
    // inputvalue(newValue, oldValue) {
    //   console.log(newValue, oldValue);
    // },
    // processValue(newValue, oldValue) {
    //   console.log(newValue, oldValue);
    // }
  },
  methods: {}
};
</script>

<style>
.calculatorresult {
  height: 90px;
  width: 200px;
  border-radius: 3px;
  margin: 10px 1 1 12px;
  position: absolute;
  border: 1px solid #000;
}

.inputup {
  height: 45px;
  width: 200px;
  border: none;
  font-size: 15px;
  text-align: right;
  outline: medium;
  /* user-select: none; */
}

.inputdown {
  height: 45px;
  width: 200px;
  border: none;
  font-size: 15px;
  text-align: right;
  outline: medium;
}
</style>