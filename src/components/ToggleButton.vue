<script setup lang="ts">
import { computed } from 'vue'

interface ToggleButtonProps {
  leftToggle: string
  rightToggle: string
  activeToggle: string
}

const props = defineProps<ToggleButtonProps>()
const emit = defineEmits(['onToggle'])

const activeBackgroundPosition = computed(() => {
  if (props.activeToggle === props.leftToggle) {
    return {
      left: 0
    }
  } else {
    return {
      left: '50%'
    }
  }
})
</script>

<template>
  <div class="toggle">
    <div class="toggleBackground">
      <div
        :class="activeToggle === leftToggle ? 'active' : 'inactive'"
        class="label"
        @click="emit('onToggle', leftToggle)"
      >
        <span>{{ leftToggle }}</span>
      </div>
      <div
        :class="activeToggle === rightToggle ? 'active' : 'inactive'"
        class="label"
        @click="emit('onToggle', rightToggle)"
      >
        <span>{{ rightToggle }}</span>
      </div>
    </div>
    <div class="activeBackground" :style="activeBackgroundPosition"></div>
  </div>
</template>

<style scoped>
@media print {
  .toggle {
    display: none;
  }
}

.toggle {
  position: relative;
  min-width: 200px;
  height: 30px;
  font-size: small;
}

.toggleBackground {
  display: grid;
  place-items: center;
  grid-template-columns: 1fr 1fr;
  background-color: #eaeaea;
  border-radius: 0.25rem;
  border: 1px solid #dedede;
  height: 100%;
}

.activeBackground {
  position: absolute;
  top: 0;
  width: 50%;
  height: 100%;
  background-color: #325b9a;
  border-radius: 0.25rem;
  transition: left 0.5s ease-in-out;
}

.label {
  display: grid;
  place-items: center;
  z-index: 1;
  height: 100%;
  width: 100%;
  cursor: pointer;
}

.inactive {
  color: #838383;
}

.active {
  color: #ffffff;
  font-weight: 500;
}
</style>
