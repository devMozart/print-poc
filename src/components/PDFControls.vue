<script setup lang="ts">
import ToggleButton from '@/components/ToggleButton.vue'
import { PageOrientation } from '@/types/PageOrientation'

interface PDFControlsProps {
  orientation: PageOrientation
}

defineProps<PDFControlsProps>()
const emit = defineEmits(['onChangeOrientation', 'onRemove', 'onDuplicate'])
</script>

<template>
  <div class="controls">
    <ToggleButton
      left-toggle="Stående"
      right-toggle="Liggande"
      :active-toggle="orientation === 'portrait' ? 'Stående' : 'Liggande'"
      @on-toggle="emit('onChangeOrientation', $event === 'Stående' ? 'portrait' : 'landscape')"
    />
    <button @click="emit('onDuplicate')">Duplicera</button>
    <button class="removeButton" @click="emit('onRemove')">Ta bort</button>
  </div>
</template>

<style scoped>
@media print {
  .controls {
    display: none;

    button {
      display: none;
    }
  }
}

.controls {
  z-index: 1;
  position: absolute;
  top: 0;
  right: -60mm;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;

  button {
    padding: 0.5rem 1rem;
    font-size: small;
    color: white;
    border: none;
    border-radius: 0.25rem;
    cursor: pointer;
    background-color: #325b9a;
  }

  .removeButton {
    background-color: #d6696b;
    margin-top: 0.5rem;
  }
}
</style>
