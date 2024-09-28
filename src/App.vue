<script setup lang="ts">
import { ref, watch } from 'vue'
const value = ref('')
const values = ref<Array<number | ''>>([])

function calculate() {
  let result = 0
  let quantity = 0
  values.value.forEach((value) => {
    if (typeof value !== 'number') return
    result += value
    quantity++
  })
  result = result / quantity || 0
  value.value = result.toString()
}

function removeEmptyFields() {
  values.value.forEach((value, index) => {
    if (value === '') values.value.splice(index, 1)
  })
}

watch(
  values,
  () => {
    calculate()
  },
  { deep: true }
)
</script>

<template>
  <div class="container">
    <header>
      <h1>Média</h1>
      <h2 v-if="value === ''">Digite um valor</h2>
      <h2 v-else>{{ value }}</h2>
    </header>
    <main>
      <div v-for="(_, index) of Array(values.length + 1)" :key="index">
        <input type="number" v-model.number="values[index]" @blur="removeEmptyFields" />
      </div>
    </main>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

* {
  font-family: 'Poppins', sans-serif;
  text-align: center;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

body {
  background: rgb(71, 181, 255);
  color: white;
}

h1 {
  font-size: 2rem;
}

h2 {
  font-size: 4rem;
}

.container {
  width: 100%;
  display: flex;
  flex-direction: column;
}

input {
  border: 2px solid rgb(60, 153, 214);
  outline: 0;
  border-radius: 12px;
  padding: 8px 0;
  font-size: 16px;
}

main {
  display: flex;
  flex-direction: row;
  gap: 24px;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  width: 100%;
}
</style>
