<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['update:tariff', 'update:isSberCustomer', 'update:rate'])

const localTariff = ref('base')
const localSberCustomer = ref(false)

const tariffs = [
  { id: 'base', name: 'Базовая', rate: '4.9' },
  { id: 'military', name: 'Военная', rate: '0.4' },
  { id: 'no-down', name: 'Без первого взноса', rate: '5.7' }
]

const updateTariff = () => {
  emit('update:tariff', localTariff.value)
  updateRate()
}

const updateSberCustomer = () => {
  localSberCustomer.value = !localSberCustomer.value
  emit('update:isSberCustomer', localSberCustomer.value)
  updateRate()
}

const updateRate = () => {
  let rate = Number(tariffs.find(t => t.id === localTariff.value).rate)
  if (localSberCustomer.value) {
    rate -= 0.5
  }
  rate = Math.max(rate, 0.1)
  emit('update:rate', rate)
}

// Инициализация начальной ставки
updateRate()
</script>

<template>
  <div class="tariff-selection">
    <label class="field-label">Тариф</label>
    
    <div class="tariff-options">
      <label 
        v-for="tariff in tariffs" 
        :key="tariff.id"
        class="tariff-option"
        :class="{ active: localTariff === tariff.id }"
      >
        <input 
          type="radio" 
          :value="tariff.id"
          v-model="localTariff"
          @change="updateTariff"
        >
        <div class="tariff-content">
          <span class="tariff-name">{{ tariff.name }}</span>
          <span class="tariff-rate">от {{ tariff.rate }}%</span>
        </div>
      </label>
    </div>

    <label class="sber-customer">
      <span>Получаю зарплату на карту Сбера</span>
      <div class="toggle-switch">
        <input 
          type="checkbox" 
          :checked="localSberCustomer"
          @change="updateSberCustomer"
        >
        <span class="toggle-slider"></span>
      </div>
    </label>
  </div>
</template>

<style lang="scss" scoped>
.tariff-selection {
  .field-label {
    display: block;
    margin-bottom: 16px;
    color: #15203D;
    font-weight: 500;
  }

  .tariff-options {
    display: flex;
    gap: 16px;
    margin-bottom: 24px;
  }

  .tariff-option {
    flex: 1;
    position: relative;
    cursor: pointer;

    input {
      position: absolute;
      opacity: 0;
      cursor: pointer;
    }

    .tariff-content {
      padding: 16px;
      border: 1px solid #E5E9F0;
      border-radius: 8px;
      display: flex;
      flex-direction: column;
      gap: 8px;
      transition: all 0.3s;

      .tariff-name {
        color: #15203D;
        font-weight: 500;
      }

      .tariff-rate {
        color: #24ed35;
        font-size: 14px;
      }
    }

    input:checked + .tariff-content {
      border-color: #24ed35;
      background: #F3FFF4;
    }
  }

  .sber-customer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #15203D;
    cursor: pointer;

    .toggle-switch {
      position: relative;
      width: 48px;
      height: 24px;

      input {
        opacity: 0;
        width: 0;
        height: 0;

        &:checked + .toggle-slider {
          background-color: #24ed35;
        }

        &:checked + .toggle-slider:before {
          transform: translateX(24px);
        }
      }

      .toggle-slider {
        position: absolute;
        cursor: pointer;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: #ccc;
        transition: .4s;
        border-radius: 24px;

        &:before {
          position: absolute;
          content: "";
          height: 20px;
          width: 20px;
          left: 2px;
          bottom: 2px;
          background-color: white;
          transition: .4s;
          border-radius: 50%;
        }
      }
    }
  }
}
</style>
