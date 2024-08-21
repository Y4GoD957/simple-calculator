<template>
  <div class="calculator">
    <input type="text" class="calculator-screen" readonly v-model="displayValue" />
    <div class="calculator-keys">
      <button class="all-clear" @click="clearAll">AC</button>
      <button @click="appendOperator('%')">%</button>
      <button @click="appendOperator('/')">/</button>

      <button @click="appendNumber('7')">7</button>
      <button @click="appendNumber('8')">8</button>
      <button @click="appendNumber('9')">9</button>
      <button @click="appendOperator('*')">*</button>

      <button @click="appendNumber('4')">4</button>
      <button @click="appendNumber('5')">5</button>
      <button @click="appendNumber('6')">6</button>
      <button @click="appendOperator('-')">-</button>

      <button @click="appendNumber('1')">1</button>
      <button @click="appendNumber('2')">2</button>
      <button @click="appendNumber('3')">3</button>
      <button @click="appendOperator('+')">+</button>

      <button class="zero-button" @click="appendNumber('0')">0</button>
      <button @click="appendComma">,</button>
      <button class="equal-sign" @click="calculate">=</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      displayValue: '', // Displays the current input or result
      currentInput: '', // Stores the current number or operator being input
      completeExpression: '', // Stores the entire expression
      lastResult: '' // Stores the last result for reuse
    };
  },
  methods: {
    appendNumber(number) {
      // Append number to the current input and expression
      if (this.currentInput === '' || /[\d,]/.test(this.currentInput)) {
        this.currentInput += number;
        this.updateDisplay();
      }
    },
    appendComma() {
      // Add comma if it doesn't already exist in the current input
      if (!this.currentInput.includes(',')) {
        this.currentInput += ',';
        this.updateDisplay();
      }
    },
    appendOperator(operator) {
      // Append operator if there is a current input
      if (this.currentInput) {
        this.completeExpression += this.currentInput + ' ' + operator + ' ';
        this.currentInput = '';
        this.updateDisplay();
      } else if (this.completeExpression) {
        // Replace the last operator with the new operator
        this.completeExpression = this.completeExpression.trim().replace(/[+\-*/%]$/, operator);
        this.updateDisplay();
      }
    },
    calculate() {
      if (this.currentInput) {
        this.completeExpression += this.currentInput;
        this.currentInput = '';
      }
      try {
        // Replace commas with dots for evaluation
        let expression = this.completeExpression.replace(/,/g, '.');
        // Handle percentage
        expression = expression.replace(/(\d+(\.\d+)?) %/, '($1 / 100)');
        // Evaluate the expression
        const result = eval(expression);
        this.lastResult = result.toString().replace('.', ','); // Store the result
        this.displayValue = this.lastResult;
        this.completeExpression = this.lastResult; // Update expression for new calculations
      } catch (error) {
        this.displayValue = 'Error';
      }
    },
    clearAll() {
      this.displayValue = '';
      this.currentInput = '';
      this.completeExpression = '';
      this.lastResult = '';
    },
    updateDisplay() {
      this.displayValue = this.completeExpression + (this.currentInput ? ' ' + this.currentInput : '');
    },
    handleKeyDown(event) {
      if (event.key === 'Backspace') {
        // Handle backspace to remove the last character
        if (this.currentInput) {
          this.currentInput = this.currentInput.slice(0, -1);
          this.updateDisplay();
        } else if (this.completeExpression) {
          this.completeExpression = this.completeExpression.trim().slice(0, -1);
          this.updateDisplay();
        }
      } else if (event.key === 'Enter') {
        this.calculate();
      } else if (!isNaN(event.key) || event.key === ',') {
        this.appendNumber(event.key);
      } else if (['+', '-', '*', '/', '%'].includes(event.key)) {
        this.appendOperator(event.key);
      }
    }
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown);
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeyDown);
  }
};
</script>

<style scoped>
.calculator {
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1);
  width: 360px; /* Larger width */
  padding: 20px;
}

.calculator-screen {
  width: 100%;
  height: 60px; /* Increased height for better display */
  background-color: #f4f4f4;
  border: none;
  border-radius: 5px;
  margin-bottom: 10px;
  text-align: right;
  padding: 10px;
  font-size: 1.5em;
  color: #333333;
  box-sizing: border-box; /* Ensures padding does not affect width */
}

.calculator-keys {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  background-color: #e0e0e0;
  border: none;
  border-radius: 5px;
  height: 60px; /* Increased height for better button size */
  font-size: 1.2em;
  color: #333333;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #d4d4d4;
}

button:active {
  background-color: #cccccc;
}

.all-clear {
  background-color: #ff6666;
  color: #ffffff;
  grid-column: span 2; /* Span two columns */
}

.equal-sign {
  background-color: #4caf50;
  color: #ffffff;
  grid-column: span 2; /* Span two columns */
}
</style>