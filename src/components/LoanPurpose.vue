<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['update:purpose'])
const purpose = ref('new')

watch(purpose, (newValue) => {
  emit('update:purpose', newValue)
})
</script>

<template>
  <div class="loan-purpose">
    <label class="field-label">Цель кредита</label>
    <select v-model="purpose" class="input-field">
      <option value="new">Готовое жильё</option>
      <option value="building">Строящееся жильё</option>
      <option value="refinance">Рефинансирование</option>
    </select>
  </div>
</template>

<style lang="scss" scoped>
.loan-purpose {
  .field-label {
    display: block;
    margin-bottom: 16px;
    color: #15203D;
    font-weight: 500;
  }

  .input-field {
    width: 100%;
    height: 48px;
    background: #F3F6FF;
    border: 1px solid transparent;
    border-radius: 8px;
    color: #15203D;
    transition: all 0.3s;
    font-size: 18px;
    font-weight: 500;

    &:hover {
      background: #dae3ff;
    }

    &:focus {
      outline: none;
      border-color: #24ed35
    }
  }
}
</style>