<template>
  <div v-cloak id="app">
    <!-- <img src="./assets/logo.png">
    <HelloWorld/>-->
    <CalculatorResult v-bind:resultshow="finallyresult" v-bind:calprocess="process"></CalculatorResult>
    <!-- <div class="beyond" v-show="recordByond == true">《</div> -->
    <div class="beyond" v-show="recordByond == true">《</div>
    <div class="buttonarea">
      <CalculatorButton @click.native="clearValue('CE')">CE</CalculatorButton>
      <CalculatorButton @click.native="clearValue('C')">C</CalculatorButton>
      <CalculatorButton @click.native="backValule()">←</CalculatorButton>
      <CalculatorButton @click.native="signum()">+|-</CalculatorButton>
      <CalculatorButton @click.native="squareValue()">√</CalculatorButton>
      <CalculatorButton @click.native="numValue('7')">7</CalculatorButton>
      <CalculatorButton @click.native="numValue('8')">8</CalculatorButton>
      <CalculatorButton @click.native="numValue('9')">9</CalculatorButton>
      <CalculatorButton @click.native="operationValue('÷')">÷</CalculatorButton>
      <CalculatorButton @click.native="percent()">%</CalculatorButton>
      <CalculatorButton @click.native="numValue('4')">4</CalculatorButton>
      <CalculatorButton @click.native="numValue('5')">5</CalculatorButton>
      <CalculatorButton @click.native="numValue('6')">6</CalculatorButton>
      <CalculatorButton @click.native="operationValue('×')">×</CalculatorButton>
      <CalculatorButton @click.native="derivative()">1/x</CalculatorButton>
      <CalculatorButton @click.native="numValue('1')">1</CalculatorButton>
      <CalculatorButton @click.native="numValue('2')">2</CalculatorButton>
      <CalculatorButton @click.native="numValue('3')">3</CalculatorButton>
      <CalculatorButton @click.native="operationValue('-')">-</CalculatorButton>
      <CalculatorButton @click.native="resultValue()" class="equal">=</CalculatorButton>
      <CalculatorButton @click.native="numValue('0')" class="zero">0</CalculatorButton>
      <CalculatorButton @click.native="numValue('.')">.</CalculatorButton>
      <CalculatorButton @click.native="operationValue('+')">+</CalculatorButton>
    </div>
  </div>
</template>

<script>
import CalculatorResult from "./components/CalculatorResult";
import CalculatorButton from "./components/CalculatorButton";

// 做一个替换字符串在中所有的方法
String.prototype.replaceAll = function(s1, s2) {
  return this.replace(new RegExp(s1, "gm"), s2);
};

Math.formatFloat = function(f, digit) {
  var m = Math.pow(10, digit);
  return parseInt(f * m, 10) / m;
};

export default {
  name: "App",
  components: {
    CalculatorResult,
    CalculatorButton
  },

  data() {
    return {
      //下层输入框内容
      finallyresult: {
        value: "0"
      },
      // 上层输入框内容
      process: {
        value: ""
      },
      //记录操作记录
      recordInput: [],

      // 标记上次输入是否为加减乘除
      isOpreation: false,

      // 标记计算器是否出错
      abnormal: false,

      // 控制操作记录显示超输入框时显示《
      recordByond: false
    };
  },

  methods: {
    // 输入数字处理
    numValue(iputNum) {
      if (this.abnormal) {
        return;
      }

      if (this.finallyresult.value.length > 16) {
        return;
      }
      // console.log('玩家点击了' + iputNum)
      let str = "" + this.finallyresult.value;
      let fdStart0 = str.indexOf("0");
      let hasPoit = str.indexOf(".");

      // 输入已经有点的情况下
      if (hasPoit != -1 && iputNum == ".") {
        return;
      }

      //首位为0
      if (fdStart0 == 0) {
        if (hasPoit == -1 && iputNum != ".") {
          //有点
          this.finallyresult.value = "";
        }
      }

      this.isOpreation = true;
      this.finallyresult.value += iputNum;
    },

    // 加减乘除运算
    operationValue(value) {
      if (this.abnormal) {
        return;
      }
      // console.log('玩家点击了' + value)
      // console.log("是否为操作符", this.isOpreation);

      if (this.isOpreation) {
        this.recordInput.push(this.finallyresult.value);
        this.finallyresult.value = "0";
        this.recordInput.push(value);
        this.isOpreation = false;
      } else {
        this.recordInput[this.recordInput.length - 1] = value;
      }
      this.showProrss();
      // console.log(this.recordInput);
    },

    // 展示输入过程
    showProrss() {
      let str = this.recordInput.join(" ");

      if (str.length > 27) {
        this.recordByond = true;
        // console.log("111");
        str = str.substring(str.length - 27, str.length - 1);
      }
      this.process.value = str;
    },

    // 回退(删减一个字符)
    backValule() {
      if (this.abnormal) {
        return;
      }

      let str1 = this.finallyresult.value;

      if (str1.length == 1) {
        this.finallyresult.value = "0";
      } else {
        this.finallyresult.value = str1.substring(0, str1.length - 1);
      }
      // console.log('玩家点击了  ←');
    },

    // 正负号
    signum() {
      if (this.abnormal) {
        return;
      }
      // console.log('玩家点击了 +|-')
      let str = this.finallyresult.value;
      this.finallyresult.value = parseFloat(this.finallyresult.value) * -1;
    },

    // 根号操作
    squareValue() {
      if (this.abnormal) {
        return;
      }

      if (parseFloat(this.finallyresult.value) < 0) {
        this.abnormal = true;
        this.finallyresult.value = "无效输入";
        return;
      } else {
        let str = this.finallyresult.value;
        this.finallyresult.value = Math.sqrt(eval(str));
      }
    },

    // 百分号操作
    percent() {
      if (this.abnormal) {
        return;
      }
      this.finallyresult.value = parseFloat(this.finallyresult.value) * 0.01;
    },

    // 导数操作
    derivative() {
      if (this.abnormal) {
        return;
      }

      let finallResult = this.finallyresult.value;
      let finallResultF = parseFloat(finallResult);

      if (finallResultF == 0) {
        this.abnormal = true;
        this.finallyresult.value = "无穷大";
        // this.clearRecordInput();
        return;
      }

      this.finallyresult.value = 1 / finallResultF;
    },

    // 计算结果
    resultValue() {
      if (this.abnormal) {
        return;
      }

      // console.log('玩家点击了 =')
      let str = this.recordInput.join(" ");
      str =
        str.replaceAll("×", "*").replaceAll("÷", "/") +
        " " +
        this.finallyresult.value;

      // console.log(str);

      //解决js计算精度问题
      let sss = Number(eval(str))
        .toFixed(16)
        .toString();
      sss = sss.split("");
      if (sss.lastIndexOf("0") > sss.indexOf(".")) {
        while (sss[sss.length - 1] == "0") {
          if (sss[sss.length - 1] != "0") {
            break;
          }
          sss = sss.splice(0, sss.length - 1);
        }
      }
      if (sss[sss.length - 1] == ".") {
        sss = sss.splice(0, sss.length - 1);
      }
      sss = sss.join("");

      this.finallyresult.value = sss;

      // console.log(this.finallyresult.value);

      if (
        this.finallyresult.value == "Infinity" ||
        this.finallyresult.value == "-Infinity"
      ) {
        this.abnormal = true;
        this.clearRecordInput();
        this.finallyresult.value = "无穷大";
        return;
      }
    },
    // 清空过程数组
    clearRecordInput() {
      this.recordInput = [];
    },
    // CE,C
    clearValue(value) {
      this.recordByond = false;
      this.abnormal = false;
      if (value == "C") {
        this.finallyresult.value = "0";
        this.clearRecordInput();
        this.showProrss();
      } else {
        this.finallyresult.value = "0";
      }
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

li {
  list-style: none;
}

[v-cloak] {
  display: none;
}
#app {
  height: 300px;
  width: 228px;
  margin: 100px auto;
  background: #f8f8f8;
  border-radius: 3px;
  position: relative;
  border: 1px solid #000;
}

.beyond {
  margin-top: 20px;
  margin-left: 10px;
  position: absolute;
}

.buttonarea {
  /* height: 190px; */
  width: 210px;
  overflow: hidden;
  margin: 120px 0 0px 8px;
  position: absolute;
  /* border: 1px solid #000; */
}
</style>
