<template lang="html">
  <main class="calculator">
    <div class="display">
      {{display}}
    </div>
    <div class="buttons">
      <div class="button-row" v-for="row in buttonRows">
        <div
          @click="buttonClicks[button.type](button)"
          :class="{ operator: button.type == 'operator' }"
          :style="button.style"
          class="button"
          v-for="button in row">
          {{button.text}}
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  methods: {
    performOperation() {
      this.display = this.operations[this.currentOperator](+this.previousValue, +this.display);
    }
  },
  data: (vm) => ({
    display: '0',
    previousValue: '',
    currentOperator: '',
    buttonClicks: {
      number(button) {
        if (vm.previousValue === null) {
          vm.previousValue = Number(vm.display);
          vm.display = '';
        }
        if (button.text == '.' && vm.display.includes('.')) return;

        vm.display += button.text;

        if (vm.display[0] == '0') {
          vm.display = vm.display.slice(1);
        }
      },
      operator(button) {
        if (vm.currentOperator) {
          vm.performOperation();
        }
        vm.previousValue = null;
        vm.currentOperator = button.text;
      },
      special(button) {
        if (button.text == 'AC') {
          vm.display = '0';
        } else if (button.text == '+/-') {
          vm.display *= -1;
        } else if (button.text == '=') {
          vm.performOperation();
          vm.currentOperator = '';
        }
      }
    },
    operations: {
      '÷': (a, b) => a / b,
      '×': (a, b) => a * b,
      '-': (a, b) => a - b,
      '+': (a, b) => a + b,
    },
    buttonRows: [
      [{
        text: 'AC',
        type: 'special'
      }, {
        text: '+/-',
        type: 'special'
      }, {
        text: '😁',
        type: 'special'
      }, {
        text: '÷',
        type: 'operator'
      }],
      [{
        text: '7',
        type: 'number'
      }, {
        text: '8',
        type: 'number'
      }, {
        text: '9',
        type: 'number'
      }, {
        text: '×',
        type: 'operator'
      }],
      [{
        text: '4',
        type: 'number'
      }, {
        text: '5',
        type: 'number'
      }, {
        text: '6',
        type: 'number'
      }, {
        text: '-',
        type: 'operator'
      }],
      [{
        text: '1',
        type: 'number'
      }, {
        text: '2',
        type: 'number'
      }, {
        text: '3',
        type: 'number'
      }, {
        text: '+',
        type: 'operator'
      }],
      [{
        text: '0',
        type: 'number'
      }, {
        text: '.',
        type: 'number',
        style: 'flex-basis: calc(100%/2)'
      }, {
        text: '=',
        type: 'special',
        style: 'flex-basis: calc(100%/2)'
      }]
    ]
  })
};
</script>

<style lang="css">
body {
  width: 100vw;
  height: 100vh;
  font-size: 4vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

.calculator {
  width: 50vw;
  height: 70vh;
  font-family: sans-serif;
  border-radius: 5px;
  overflow: hidden;
  border: 0.5px solid grey;
}

.display {
  height: 10vh;
  background: #989898;
  text-align: right;
  color: white;
  font-size: 1.5em;
}

.buttons {
  height: 60vh;
  text-align: center;
}

/* ​row: display: flex; flex-wrap: wrap; 0 button: flex-basis: calc(100% /2); */

.button-row {
  height: 20%;
  width: 100%;
  display: flex;
  justify-content: space-around;
}

.button {
  display: inline-block;
  outline: 0.5px solid grey;
  width: 100%;
  background: #D0D0D0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.operator {
  color: white;
  background: #F6872C;
}

.button:active {
  background: #b5b4b3;
}
</style>
