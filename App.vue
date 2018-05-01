<template lang="html">
  <main class="calculator">
    <div ref="display" class="display">
      <div class="display-text">
        <span ref="displayText">{{display}}</span>
      </div>
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
    updateDisplay(value) {
      this.display = value;
      this.$nextTick(() => {
        let fontSize = 4;
        this.$refs.display.setAttribute('style', 'font-size:4vw');
        while (this.$refs.displayText.offsetWidth + 30 > this.$refs.display.offsetWidth) {
          this.$refs.display.setAttribute('style', 'font-size:' + fontSize + 'vw');
          fontSize -= 0.1;

          if (fontSize <= 0.1) break;
        }
      })
    },
    performOperation() {
      if (this.previousValue) {
        const value = this.operations[this.currentOperator](+this.previousValue, +this.display);
        this.updateDisplay(value);
      }
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
          vm.updateDisplay('');
        }
        if (button.text == '.' && vm.display.includes('.')) return;

        vm.updateDisplay(vm.display + button.text);

        if (vm.display.length > 1 && vm.display[0] == '0' && vm.display[1] != '.') {
          vm.updateDisplay(vm.display.slice(1));
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
          vm.updateDisplay('0');
        } else if (button.text == '+/-') {
          vm.updateDisplay(vm.display * -1);
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
        type: 'special',
        style: 'flex-basis: calc(100%/2)'
      }, {
        text: '÷',
        type: 'operator',
        style: 'flex-basis: calc(100%/2)'
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
  font-size: 2.5vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

.calculator {
  width: 30vw;
  height: calc(30vw * 1.4);
  font-family: sans-serif;
  border-radius: 5px;
  overflow: hidden;
  border: 0.5px solid grey;
}

.display {
  height: calc(30vw * 0.2);
  background: #989898;
  color: white;
  font-size: 4vw;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

.display-text {
  padding: 0.5vw;
  text-align: right;
}

.buttons {
  height: calc(30vw * 1.2);
  text-align: center;
}

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
