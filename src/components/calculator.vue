<template>
    <section id="calculator">
        <div class="top">
            <div id="input">{{expression}}</div>>
        </div>
        <div class="bottom">
            <div class="left">
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">7</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">8</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">9</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">4</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">5</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">6</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">1</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">2</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">3</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">.</div></button>
                <button class="digit" v-on:click="clickButt($event)"><div class="inside-button">0</div></button>
                <button class="digit" v-on:click="clearScreen"><div class="inside-button" id="del-div">DEL</div></button>
            </div>
            <div class="right">
                <button class="operators" v-on:click="clickButt($event)"><div class="inside-button">/</div></button>
                <button class="operators" v-on:click="clickButt($event)"><div class="inside-button">*</div></button>
                <button class="operators" v-on:click="clickButt($event)"><div class="inside-button">-</div></button>
                <button class="operators" v-on:click="clickButt($event)"><div class="inside-button">+</div></button>
                <button class="operators" v-on:click="calculate"><div class="inside-button">=</div></button>
            </div>
        </div>
    </section>
</template>

<script>
  export default {
    name: "calculator",
    data() {
      return {
        expression: '',
        operations: [],
        numbers: [],
        operands: ['+', '-', '*', '/'],
        digits: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
      }
    },
    methods: {
        clickButt(event) {
          this.expression += event.target.textContent;
        },

        clearScreen() {
          this.expression = '';
        },

      calculate() {
          this.parseExpression();
          this.expression = this.getResult;
      },

      parseExpression() {
        let value = '';

        if (this.operands.indexOf(this.expression[this.expression.length - 1]) !== -1) {
          this.expression = this.expression.slice(0, this.expression.length - 1);
        }

        for (let i = 0; i < this.expression.length; i++) {
          if (this.operands.indexOf(this.expression[i]) !== -1) {
            this.operations.push(this.expression[i]);
            this.numbers.push(Number(value));
            value = '';
          }

          if (this.digits.indexOf(this.expression[i]) !== -1  || this.expression[i] === '.') {
            value += this.expression[i];
          }
        }

        if (value !== '') {
          this.numbers.push(Number(value));
        }
      }
    },
    computed: {
      getResult() {
        let firstNumber = this.numbers[0];
        let secondNumber = this.numbers[1];

        for (let i = 0; i < this.numbers.length; i++) {

          switch (this.operations[i]) {
            case '+' :
              firstNumber += secondNumber;
              break;

            case '-' :
              firstNumber -= secondNumber;
              break;

            case '*' :
              firstNumber *= secondNumber;
              break;

            case '/' :
              if (secondNumber !== 0) {
                firstNumber /= secondNumber;
              } else
                return 'error';
              break;
          }
          secondNumber = this.numbers[i + 2];
        }
        return firstNumber;
      }
    }
  }
</script>

<style scoped>
    * {
        margin: 0;
        padding: 0;
        font-family: Arial, Helvetica, sans-serif;
    }

    *:focus {
        outline: none;
    }

    body {
        background: gray;
    }

    ul {
        list-style: none;
        text-align: center;
    }

    #calculator {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        width: 375px;
        height: 650px;
        background: #000;
        display: grid;
        grid-template-rows: 1fr 2fr;
    }

    .top {
        background: #121212;
        position: relative;
    }

    .bottom {
        display: grid;
        grid-template-rows: 1fr;
        grid-template-columns: 3fr 1fr;
        background: linear-gradient(180deg, rgba(37,22,22,1) 0%, rgba(0,0,0,1) 100%);
    }

    .left {
        display: grid;
        grid-template-rows: repeat(4, 1fr);
        grid-template-columns: repeat(3, 1fr);
    }

    .right {
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: repeat(5, 1fr);
    }

    button {
        border: none;
        background: none;
        color: rgb(202, 202, 202);
        border-radius: 50%;
        font-size: 24px;
        border-top: 50px;
        animation-direction: normal;
        margin: auto;
        width: 100%;
        height: 100%;
        position: relative;
    }

    .inside-button {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        border-radius: 50%;
        line-height: 50px;
        transition: .5s;
        height: 50px;
        width: 50px;
    }

    #del-div {
        height: 70px !important;
        width: 70px !important;
        line-height: 70px;
    }

    button:hover .inside-button {
        background: rgba(38, 73, 73, 0.9);
    }

    .operators:hover {

    }

    .operators {
        color: #3FD7D5;
        font-size: 24px;
        font-weight: bold;
    }

    #input {
        position: absolute;
        left: 5%;
        bottom: 15%;
        width: 90%;
        height: 100px;
        font-size: 50px;
        text-align: right;
        color: #DDDDDD;
        background: none;
        border: none;
    }
</style>