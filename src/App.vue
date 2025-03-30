<script setup>
import { ref, watch } from 'vue'
import LoanPurpose from './components/LoanPurpose.vue'
import TariffSelection from './components/TariffSelection.vue'
import PropertyCost from './components/PropertyCost.vue'
import DownPayment from './components/DownPayment.vue'
import LoanTerm from './components/LoanTerm.vue'
import CalculationResults from './components/CalculationResults.vue'

const loanPurpose = ref('new')
const selectedTariff = ref('base')
const isSberCustomer = ref(false)
const propertyCost = ref(334000)
const downPayment = ref(33734)
const useMaternityCapital = ref(false)
const loanTerm = ref(1)
const interestRate = ref(4.9)
const loanAmount = ref(0)
const monthlyPayment = ref(0)

const calculateMonthlyPayment = () => {
  const rate = interestRate.value / 12 / 100
  const months = loanTerm.value * 12
  monthlyPayment.value = loanAmount.value * 
    (rate / (1 - Math.pow(1 + rate, -months)))
}

const calculateLoan = () => {
  let totalDownPayment = downPayment.value
  if (useMaternityCapital.value) {
    totalDownPayment += 524527.90 // Размер маткапитала на 2024 год
  }
  loanAmount.value = Math.max(0, propertyCost.value - totalDownPayment)
  calculateMonthlyPayment()
}

const updateInterestRate = (rate) => {
  interestRate.value = rate
  calculateMonthlyPayment()
}

const handleSubmit = () => {
  const result = {
    interest_rate: interestRate.value,
    loan_amount: loanAmount.value,
    bank_customer: isSberCustomer.value,
    loan_term: loanTerm.value,
  }
  
  if (useMaternityCapital.value) {
    result.maternity_capital = 524527.90
    result.personal_funds = downPayment.value
  } else {
    result.initial_payment = downPayment.value
  }
  
  console.log(result)
}

watch([propertyCost, downPayment, useMaternityCapital], () => {
  calculateLoan()
})

calculateLoan()
</script>

<template>
  <div class="container">
    <div class="mortgage-calculator">
      <h1 class="title">Калькулятор ипотечного кредита</h1>
      
      <div class="calculator-content">
        <div class="calculator-form">
          <LoanPurpose
            v-model:purpose="loanPurpose"
          />

          <TariffSelection
            v-model:tariff="selectedTariff"
            v-model:isSberCustomer="isSberCustomer"
            @update:rate="updateInterestRate"
          />

          <PropertyCost
            v-model:cost="propertyCost"
            @update:cost="calculateLoan"
          />

          <DownPayment
            v-model:payment="downPayment"
            v-model:useMaternityCapital="useMaternityCapital"
            :propertyCost="propertyCost"
            @update:payment="calculateLoan"
          />

          <LoanTerm
            v-model:term="loanTerm"
            @update:term="calculateLoan"
          />
        </div>

        <CalculationResults
          :interestRate="interestRate"
          :loanAmount="loanAmount"
          :monthlyPayment="monthlyPayment"
          @submit="handleSubmit"
        />
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@use './assets/styles/variables' as *;

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  color: $text-primary;
  line-height: 1.5;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
}

.mortgage-calculator {
  background: white;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  overflow: hidden;

  .title {
    font-size: 28px;
    font-weight: 600;
    color: $text-primary;
    padding: 24px;
    border-bottom: 1px solid #F3F6FF;
  }

  .calculator-content {
    display: grid;
    grid-template-columns: 1fr 400px;
    gap: 24px;
    padding: 24px;
  }

  .calculator-form {
    display: grid;
    gap: 24px;
  }
}

.field-label {
  display: block;
  margin-bottom: 8px;
  color: $text-primary;
  font-weight: 500;
}

.input-field {
  width: 100%;
  height: 48px;
  background: $input-bg;
  border: 1px solid transparent;
  border-radius: 8px;
  padding: 0 16px;
  color: $text-primary;
  transition: all 0.3s;

  &:hover {
    background: $input-hover;
  }

  &:focus {
    outline: none;
    border-color: $primary-color;
  }
}

.slider {
  width: 100%;
  height: 6px;
  background: $input-bg;
  border-radius: 3px;
  margin: 16px 0;
  appearance: none;
  -webkit-appearance: none;

  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 20px;
    height: 20px;
    background: $primary-color;
    border-radius: 50%;
    cursor: pointer;
  }
}
</style>
