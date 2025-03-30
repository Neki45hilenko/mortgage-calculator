<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  cost: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['update:cost'])

const localCost = ref(props.cost)

const validateAndUpdate = () => {
  let value = Number(localCost.value)
  if (value < 334000) value = 334000
  if (value > 100000000) value = 100000000
  localCost.value = value
  emit('update:cost', value)
}

watch(() => props.cost, (newValue) => {
  localCost.value = newValue
})

const handleInput = (event) => {
  validateAndUpdate()
}
</script>

<template>
  <div class="property-cost">
    <label class="field-label">Стоимость недвижимости</label>
    
    <div class="input-group">
      <input 
        type="number" 
        v-model="localCost"
        class="input-field"
        min="334000"
        max="100000000"
        @input="validateAndUpdate"
      >
      <span class="currency">₽</span>
    </div>

    <input 
      type="range"
      v-model="localCost"
      class="slider property-cost-slider"
      min="334000"
      max="100000000"
      step="1000"
      @input="handleInput"
    >

    <div class="range-labels">
      <span>от 334 тыс. ₽</span>
      <span>до 100 млн ₽</span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.property-cost {
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

  .property-cost-slider {
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
}
</style>
