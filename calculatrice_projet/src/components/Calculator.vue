<template>
  <div class="calculator-container">
    <h1>Calculatrice Interactive</h1>
    <form @submit.prevent="calculate">
      <div class="mb-3">
        <label class="form-label">Choisissez une opération :</label>
        <div>
          <div class="form-check-inline">
            <input class="form-check-input" type="radio" id="addition" value="+" v-model="operation">
            <label class="form-check-label" for="addition">Addition</label>
          </div>
          <div class="form-check-inline">
            <input class="form-check-input" type="radio" id="soustraction" value="-" v-model="operation">
            <label class="form-check-label" for="soustraction">Soustraction</label>
          </div>
          <div class="form-check-inline">
            <input class="form-check-input" type="radio" id="multiplication" value="*" v-model="operation">
            <label class="form-check-label" for="multiplication">Multiplication</label>
          </div>
          <div class="form-check-inline">
            <input class="form-check-input" type="radio" id="division" value="/" v-model="operation">
            <label class="form-check-label" for="division">Division</label>
          </div>
        </div>
      </div>

      <div class="valeur">
        <label class="form-label">Valeur 1 :</label>
        <input type="number" class="form-control" v-model.number="value1" required>
      </div>

      <div class="valeur">
        <label class="form-label">Valeur 2 :</label>
        <input type="number" class="form-control" v-model.number="value2" required>
      </div>

      <button type="submit" class="btn-primary ">Calculer</button>

      <div v-if="errorMessage" class="alert alert-danger mt-3">
        {{ errorMessage }}
      </div>
      <div v-if="result !== null" class="alert alert-success mt-3">
        Résultat : {{ result }}
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const operation = ref('');
const value1 = ref(null);
const value2 = ref(null);
const result = ref(null);
const errorMessage = ref('');

function calculate() {
  errorMessage.value = '';
  result.value = null;

  if (operation.value && value1.value !== null && value2.value !== null) {
    try {
      switch (operation.value) {
        case '+':
          result.value = value1.value + value2.value;
          break;
        case '-':
          result.value = value1.value - value2.value;
          break;
        case '*':
          result.value = value1.value * value2.value;
          break;
        case '/':
          if (value2.value === 0) {
            throw new Error('Division par zéro interdite.');
          }
          result.value = value1.value / value2.value;
          break;
        default:
          throw new Error('Opération non valide.');
      }
    } catch (e) {
      errorMessage.value = e.message;
    }
  } else {
    errorMessage.value = 'Veuillez remplir tous les champs.';
  }
}
</script>

<style scoped>
.calculator-container {
  max-width: 1200px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f8f9fa;
 

}

h1 { 
  font-size: 24px;
}

.btn-primary {
  background-color: #007bff;
  border-color: #007bff;
  transition: background-color 0.3s, border-color 0.3s;
}

.btn-primary:hover {
  background-color: #0056b3;
  border-color: #004085;
}
</style>
