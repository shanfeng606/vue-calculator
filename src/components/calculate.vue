<template>
  <div class="calculator">
    <div class="result" style="grid-area:result">{{ equation }}</div>
    <button style="grid-area:ac" @click="clear">AC</button>
    <button style="grid-area:plus-minus" @click="calculateToggle">±</button>
    <button style="grid-area:percent" @click="calculatePercentage">%</button>
    <button style="grid-area:add" @click="append('+')">+</button>
    <button style="grid-area:subtract" @click="append('-')">-</button>
    <button style="grid-area:multiply" @click="append('×')">×</button>
    <button style="grid-area:divide" @click="append('÷')">÷</button>
    <button style="grid-area:equal" @click="calculate">=</button>

    <button style="grid-area:number-1" @click="append(1)">1</button>
    <button style="grid-area:number-2" @click="append(2)">2</button>
    <button style="grid-area:number-3" @click="append(3)">3</button>
    <button style="grid-area:number-4" @click="append(4)">4</button>
    <button style="grid-area:number-5" @click="append(5)">5</button>
    <button style="grid-area:number-6" @click="append(6)">6</button>
    <button style="grid-area:number-7" @click="append(7)">7</button>
    <button style="grid-area:number-8" @click="append(8)">8</button>
    <button style="grid-area:number-9" @click="append(9)">9</button>
    <button style="grid-area:number-0" @click="append(0)">0</button>
    <button style="grid-area:dot" @click="append('.')">.</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      equation: '0',
      isDecimalAdded: false,
      isOperatorAdded: false,
      isStarted: false,
    };
  },
  methods: {
    //输入+ - x /
    isOperator(character) {
      return ["+", "-", "×", "÷"].indexOf(character) > -1;
    },
    // operators or Numbers
    append(character) {
      if (this.equation.length >= 10) {
        alert("超出长度限制");
        this.clear();
        return;
      }
      //首次输入时不为操作符
      if (this.equation === "0" && !this.isOperator(character)) {
        if (character === ".") {
          this.equation += "" + character; //引号的作用将其转换成字符串
          this.isDecimalAdded = true;
        } else {
          this.equation = "" + character;
        }
        this.isStarted = true;
        return;
      }

      //if NUmber
      if (!this.isOperator(character)) {
        //防止连续输入.
        if (character === "." && this.isDecimalAdded) {
          return;
        }
        if (character === ".") {
          this.isDecimalAdded = true;
          this.isOperatorAdded = true;
        } else {
          this.isOperatorAdded = false;
        }
        this.equation += "" + character;
      }

      //Add Operator
      if (this.isOperator(character) && !this.isOperatorAdded) {
        this.equation += "" + character;
        this.isDecimalAdded = false;
        this.isOperatorAdded = true;
      }
    },
    //=
    calculate() {
      let result = this.equation
        .replace(new RegExp("×", "g"), "*")
        .replace(new RegExp("÷", "g"), "/");

      this.equation = parseFloat(eval(result).toFixed(9)).toString();
      this.isDecimalAdded = false;
      this.isOperatorAdded = false;
    },
    //正负号+/-
    calculateToggle() {
      if (this.isOperatorAdded || !this.isStarted) {
        return;
      }
      this.equation = this.equation + "* -1";
      this.calculate();
    },
    //%
    calculatePercentage() {
      if (this.isOperatorAdded || !this.isStarted) {
        return;
      }
      this.equation = this.equation + "* 0.01";
      this.calculate();
    },
    //AC
    clear() {
      (this.equation = "0"),
        (this.isDecimalAdded = false),
        (this.isOperatorAdded = false),
        (this.isStarted = false);
    },
  },
};
</script>

<style lang="css" scoped>
.calculator {
  --button-width: 80px;
  --button-height: 80px;

  display: grid;
  grid-template-areas:
    "result result result result"
    "ac plus-minus percent divide"
    "number-7 number-8 number-9 multiply"
    "number-4 number-5 number-6 subtract"
    "number-1 number-2 number-3 add"
    "number-0 number-0 dot equal";

  grid-template-columns: repeat(4, var(--button-width));
  grid-template-rows: repeat(6, var(--button-height));

  box-shadow: -8px -8px 16px -10px rgba(255, 255, 255, 1),
    8px 8px 16px -10px rgba(0, 0, 0, 0.15);
  padding: 24px;
  border-radius: 20px;
}
.calculator button {
  margin: 8px;
  padding: 0;
  border: 0;
  display: block;
  outline: none;
  border-radius: calc(var(--button-height) / 2);
  font-size: 24px;
  font-family: Helvetica;
  color: #999;
  background: linear-gradient(
    135deg,
    rgba(230, 230, 230, 1) 0%,
    rgba(246, 246, 246, 1) 100%
  );
  box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1),
    4px 4px 10px -8px rgba(0, 0, 0, 0.3);
}
.calculator button:active {
  box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1) inset,
    4px 4px 10px -8px rgba(0, 0, 0, 0.3) inset;
}

.result {
  text-align: right;
  line-height: var(--button-height);
  font-size: 48px;
  font-family: Arial, Helvetica, sans-serif;
  padding: 0 20px;
  color: #666;
}
</style>
