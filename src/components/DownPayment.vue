<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  payment: {
    type: Number,
    required: true
  },
  propertyCost: {
    type: Number,
    required: true
  },
  useMaternityCapital: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:payment', 'update:useMaternityCapital'])

const localPayment = ref(props.payment)
const localMaternityCapital = ref(props.useMaternityCapital)

const minPayment = computed(() => Math.ceil(props.propertyCost * 0.101))

const validateAndUpdate = () => {
  let value = Number(localPayment.value)
  if (value < minPayment.value) value = minPayment.value
  if (value > props.propertyCost) value = props.propertyCost
  localPayment.value = value
  emit('update:payment', value)
}

const updateMaternityCapital = () => {
  localMaternityCapital.value = !localMaternityCapital.value
  emit('update:useMaternityCapital', localMaternityCapital.value)
}

const formatCurrency = (value) => {
  return new Intl.NumberFormat('ru-RU', {
    style: 'currency',
    currency: 'RUB',
    maximumFractionDigits: 0
  }).format(value)
}

const handleInput = (event) => {
  validateAndUpdate()
}

watch(() => props.payment, (newValue) => {
  localPayment.value = newValue
})

watch(() => props.propertyCost, () => {
  validateAndUpdate()
})

watch(() => props.useMaternityCapital, (newValue) => {
  localMaternityCapital.value = newValue
})
</script>

<template>
  <div class="down-payment">
    <label class="field-label">Первоначальный взнос</label>
    
    <div class="input-group">
      <input 
        type="number" 
        v-model="localPayment"
        class="input-field"
        :min="minPayment"
        :max="propertyCost"
        @input="validateAndUpdate"
      >
      <span class="currency">₽</span>
    </div>

    <input 
      type="range"
      v-model="localPayment"
      class="slider down-payment-slider"
      :min="minPayment"
      :max="propertyCost"
      step="1000"
      @input="handleInput"
    >

    <div class="range-labels">
      <span>{{ (localPayment / propertyCost * 100).toFixed(1) }}%</span>
      <span>{{ formatCurrency(localPayment) }}</span>
    </div>

    <label class="maternity-capital">
      <span>Использовать материнский капитал</span>
      <div class="toggle-switch">
        <input 
          type="checkbox" 
          :checked="localMaternityCapital"
          @change="updateMaternityCapital"
        >
        <span class="toggle-slider"></span>
      </div>
    </label>
  </div>
</template>

<style lang="scss" scoped>
.down-payment {
  .field-label {
    display: block;
    margin-bottom: 16px;
    color: #15203D;
    font-weight: 500;
  }

  .input-group {
    position: relative;
    margin-bottom: 16px;

    .input-field {
      width: 100%;
      height: 48px;
      background: #F3F6FF;
      border: 1px solid transparent;
      border-radius: 8px;
      padding: 0 48px 0 16px;
      color: #15203D;
      transition: all 0.3s;

      &:hover {
        background: #dae3ff;
      }

      &:focus {
        outline: none;
        border-color: #24ed35
      }
    }

    .currency {
      position: absolute;
      right: 16px;
      top: 50%;
      transform: translateY(-50%);
      color: #93989d;
    }
  }

  .down-payment-slider {
    width: 100%;
    height: 6px;
    background: #F3F6FF;
    border-radius: 3px;
    margin: 16px 0;
    appearance: none;
    -webkit-appearance: none;

    &::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 20px;
      height: 20px;
      background: #24ed35;
      border-radius: 50%;
      cursor: pointer;
    }
  }

  .range-labels {
    display: flex;
    justify-content: space-between;
    color: #93989d;
    font-size: 14px;
  }

  .maternity-capital {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 16px;
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
