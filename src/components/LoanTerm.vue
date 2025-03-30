<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  term: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['update:term'])

const localTerm = ref(props.term)

const validateAndUpdate = () => {
  let value = Number(localTerm.value)
  if (value < 1) value = 1
  if (value > 30) value = 30
  localTerm.value = value
  emit('update:term', value)
}

watch(() => props.term, (newValue) => {
  localTerm.value = newValue
})

const handleInput = (event) => {
  validateAndUpdate()
}
</script>

<template>
  <div class="loan-term">
    <label class="field-label">Срок кредита</label>
    
    <div class="input-group">
      <input 
        type="number" 
        v-model="localTerm"
        class="input-field"
        min="1"
        max="30"
        @input="validateAndUpdate"
      >
      <span class="years">лет</span>
    </div>

    <input 
      type="range"
      v-model="localTerm"
      class="slider loan-term-slider"
      min="1"
      max="30"
      step="1"
      @input="handleInput"
    >

    <div class="range-labels">
      <span>1 год</span>
      <span>30 лет</span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.loan-term {
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

    .years {
      position: absolute;
      right: 16px;
      top: 50%;
      transform: translateY(-50%);
      color: #93989d;
    }
  }

  .loan-term-slider {
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
