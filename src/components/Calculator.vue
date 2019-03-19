<template>
  <div id="Calculator">
    <div class="display">{{expression}}</div>
    <div v-on:click='pressClear'      class="btn operator">C</div>
    <div v-on:click='pressDelete'     class="btn operator">del</div>
    <div v-on:click='pressPercent'    class="btn operator">%</div>
    <div v-on:click='pressSquareRoot' class="btn operator">√</div>
    <div v-on:click='pressNumber'     class="btn">7</div>
    <div v-on:click='pressNumber'     class="btn">8</div>
    <div v-on:click='pressNumber'     class="btn">9</div>
    <div v-on:click='pressOperator'   class="btn operator">+</div>
    <div v-on:click='pressNumber'     class="btn">4</div>
    <div v-on:click='pressNumber'     class="btn">5</div>
    <div v-on:click='pressNumber'     class="btn">6</div>
    <div v-on:click='pressOperator'   class="btn operator">-</div>
    <div v-on:click='pressNumber'     class="btn">1</div>
    <div v-on:click='pressNumber'     class="btn">2</div>
    <div v-on:click='pressNumber'     class="btn">3</div>
    <div v-on:click='pressOperator'   class="btn operator">*</div>
    <div v-on:click='pressNumber'     class="btn">0</div>
    <div v-on:click='pressDot'        class="btn operator">.</div>
    <div v-on:click='pressResult'     class="btn operator">=</div>
    <div v-on:click='pressOperator'   class="btn operator">/</div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  created: function() {
    window.addEventListener("keydown", this.onKeyPress);
  },
  beforeDestroy: function() {
    window.removeEventListener("keydown", this.onKeyPress);
  },
  data() {
    return {
      expression: "0",
      currentNumber: "0"
    };
  },
  methods: {
    pressNumber(event) {
      if (this.expression === "0") {
        this.expression = "";
        this.currentNumber = ""
      }
      let number = event.target.innerText;
      this.expression += number;
      this.currentNumber += number;
    },
    pressOperator(event) {
      let lastIdx = this.expression.length - 1;
      let operator = event.target.innerText;
      //Allow to change the sign of a number
      if (this.expression === "0" && operator === "-") {
        this.expression = operator;
      }
      //Don't allow two operators in a row or an operator after a dot
      if (!this.expression[lastIdx].match(/[/+*-.]/)) {
        this.expression += operator;
        this.currentNumber = "";
      }
      this.expression = this.expression.slice(0, -1) + operator;
    },
    pressResult() {
      this.expression = eval(this.expression).toString();
      this.$emit("api-call");
    },
    pressClear() {
      this.expression = "0";
    },
    pressDelete() {
      if (this.expression) this.expression = this.expression.slice(0, -1);
    },
    pressPercent() {
      this.expression = (eval(this.expression) / 100).toString();
    },
    pressSquareRoot() {
      this.expression = Math.sqrt(eval(this.expression));
    },
    pressDot() {
      let lastIdx = this.expression.length - 1;
      //Don't allow more than one dot in the same number or after an operator
      if (
        !this.currentNumber.includes(".") &&
        !this.expression[lastIdx].match(/[/+*-]/)
      ) {
        this.expression += ".";
        this.currentNumber += ".";
      }
    },
    buildEvent(keyPressed) {
      let newEvent = {};
      newEvent["target"] = {};
      newEvent["target"]["innerText"] = keyPressed;
      return newEvent;
    },
    onKeyPress(event) {
      
      if (event.key === "Enter") {
        this.pressResult();
      } else if (event.key.match(/[0-9]/)) {
        this.pressNumber(this.buildEvent(event.key));
      } else if (event.key.match(/[/+*-]/)) {
        this.pressOperator(this.buildEvent(event.key));
      } else if (event.key === ".") {
        this.pressDot();
      } else if (event.key === "√") {
        this.pressSquareRoot();
      } else if (event.key === "Backspace") {
        this.pressDelete();
      } else if (event.key === "c") {
        this.pressClear();
      }
    }
  }
};
</script>

<style lang="scss">
#Calculator {
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  width: 400px;
  background-color: rgb(152, 161, 172);
  border-radius: 10%;
  padding: 3%;
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
  border-radius: 10%;
  padding: 1%;
  margin-bottom: 3%;
}
.btn {
  background-color: #f2f2f2;
  border: 1px solid #999;
  border-radius: 30%;
  text-align: center;
  margin: 2%;
}

.operator {
  background-color: orange;
}
</style>
