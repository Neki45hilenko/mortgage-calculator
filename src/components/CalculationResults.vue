<script setup>
defineProps({
  interestRate: {
    type: Number,
    required: true
  },
  loanAmount: {
    type: Number,
    required: true
  },
  monthlyPayment: {
    type: Number,
    required: true
  }
})

defineEmits(['submit'])

const formatCurrency = (value) => {
  return new Intl.NumberFormat('ru-RU', {
    style: 'currency',
    currency: 'RUB',
    maximumFractionDigits: 0
  }).format(value)
}
</script>

<template>
  <div class="calculation-results">
    <div class="results-grid">
      <div class="result-item">
        <span class="label">Процентная ставка</span>
        <span class="value">{{ interestRate.toFixed(1) }}%</span>
      </div>
      
      <div class="result-item">
        <span class="label">Ежемесячный платеж</span>
        <span class="value">{{ formatCurrency(monthlyPayment) }}</span>
      </div>
      
      <div class="result-item full-width">
        <span class="label">Сумма кредита</span>
        <span class="value">{{ formatCurrency(loanAmount) }}</span>
      </div>
    </div>

    <button class="submit-button" @click="$emit('submit')">
      Получить одобрение
    </button>
  </div>
</template>

<style lang="scss" scoped>
.calculation-results {
  background: #F3F6FF;
  padding: 24px;
  border-radius: 8px;
  height: fit-content;

  .results-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
    margin-bottom: 24px;
  }

  .result-item {
    display: flex;
    flex-direction: column;
    gap: 8px;

    &.full-width {
      grid-column: 1 / -1;
    }

    .label {
      color: #93989d;
      font-size: 14px;
    }

    .value {
      font-size: 24px;
      font-weight: 600;
      color: #15203D;
    }
  }

  .submit-button {
    width: 100%;
    height: 48px;
    background: #24ed35;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s;

    &:hover {
      background: #1ed32a;
    }
  }
}
</style>
