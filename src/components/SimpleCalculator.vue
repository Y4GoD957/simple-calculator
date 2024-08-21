<template>
  <div class="calculator">
    <input type="text" v-model="display" disabled />

    <div class="buttons">
      <button @click="clear">C</button>
      <button @click="append('(')">(</button>
      <button @click="append(')')">)</button>
      <button @click="operate('/')">/</button>

      <button @click="append('7')">7</button>
      <button @click="append('8')">8</button>
      <button @click="append('9')">9</button>
      <button @click="operate('*')">*</button>

      <button @click="append('4')">4</button>
      <button @click="append('5')">5</button>
      <button @click="append('6')">6</button>
      <button @click="operate('-')">-</button>

      <button @click="append('1')">1</button>
      <button @click="append('2')">2</button>
      <button @click="append('3')">3</button>
      <button @click="operate('+')">+</button>

      <button @click="append('0')" class="zero">0</button>
      <button @click="append('.')">.</button>
      <button @click="calculate">=</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: '',
    };
  },
  methods: {
    append(character) {
      this.display += character;
    },
    clear() {
      this.display = '';
    },
    operate(operator) {
      this.display += ` ${operator} `;
    },
    calculate() {
      try {
        this.display = eval(this.display.replace(/[^-()\d/*+.]/g, ''));
      } catch {
        this.display = 'Operação Inválida!';
      }
    },
  },
};
</script>

<style scoped>
.calculator {
  width: 320px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f1f1f1;
  border-radius: 10px;
}

input[type="text"] {
  width: 100%;
  height: 40px;
  text-align: right;
  margin-bottom: 10px;
  font-size: 24px;
  border-radius: 5px;
  border: 1px solid #ccc;
  padding: 5px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  height: 50px;
  font-size: 18px;
  border-radius: 5px;
  border: none;
  background-color: #ddd;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #bbb;
}

.zero {
  grid-column: span 2;
}
</style>