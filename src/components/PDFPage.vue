<script setup lang="ts">
import { computed, ref } from 'vue'
import PDFControls from '@/components/PDFControls.vue'
import { PageOrientation } from '@/types/PageOrientation'
import { PageSize } from '@/types/PageSize'
import { Offer } from '@/types/Offer'

interface PDFPageProps {
  size: PageSize
  orientation: PageOrientation
  offer: Offer
}

const props = defineProps<PDFPageProps>()
const emit = defineEmits(['onRemove', 'onDuplicate'])

const orientation = ref<PageOrientation>(props.orientation)

const orientationClass = computed(() => {
  if (props.size === 'A3')
    return orientation.value === 'portrait' ? 'portrait-page-A3' : 'landscape-page-A3'
  else return orientation.value === 'portrait' ? 'portrait-page' : 'landscape-page'
})
</script>

<template>
  <div class="wrapper">
    <PDFControls
      :orientation="orientation"
      @on-change-orientation="orientation = $event"
      @on-duplicate="emit('onDuplicate')"
      @on-remove="emit('onRemove')"
    />
    <section class="sheet" :class="orientationClass">
      <div class="offer">
        <img :src="offer.image" />
        <div class="price-container">
          <div class="price">
            <div class="price-splash"></div>
            <div class="price-text">
              {{ offer.price }}
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
@page {
  size: A4;
  margin: 0;
}

@page orientation-portrait {
  size: portrait;
}

@page orientation-landscape {
  size: landscape;
}

@media print {
  .portrait-page {
    width: 210mm;
    height: 297mm;
  }
  .landscape-page {
    width: 297mm;
    height: 210mm;
  }
  .portrait-page-A3 {
    width: 297mm;
    height: 420mm;
  }
  .landscape-page-A3 {
    width: 420mm;
    height: 297mm;
  }

  button {
    display: none;
  }
}

@media screen {
  .sheet {
    box-shadow: 0 0.5mm 2mm rgba(0, 0, 0, 0.3);
    margin: 5mm auto;
  }
}

.wrapper {
  position: relative;
}

.landscape-page {
  page: orientation-landscape;
  width: 297mm;
  height: 210mm;
}

.portrait-page {
  page: orientation-portrait;
  width: 210mm;
  height: 297mm;
}

.portrait-page-A3 {
  page: orientation-portrait;
  width: 297mm;
  height: 420mm;
}
.landscape-page-A3 {
  page: orientation-landscape;
  width: 420mm;
  height: 297mm;
}

.sheet {
  margin: 0;
  padding: 5mm;
  overflow: hidden;
  position: relative;
  box-sizing: border-box;
  page-break-after: always;
  background-color: #a9c9ff;
  background-image: linear-gradient(180deg, #a9c9ff 0%, #ffbbec 100%);
  display: grid;
  place-items: center;

  transition: all 0.5s ease-in-out 0.5s;
}

.offer {
  position: relative;

  img {
    max-width: 180mm;
  }
}

.price-container {
  position: absolute;
  bottom: 0;
  right: 0;
}

.price {
  position: relative;
  padding: 1rem;
  min-height: 110px;
  min-width: 140px;
  display: grid;
  place-items: center;

  .price-splash {
    position: absolute;
    background-color: #ffec03;
    border-radius: 50%;
    height: 110px;
    width: 140px;
  }

  .price-text {
    position: absolute;
    color: #f30d16;
    font-size: 40px;
    font-weight: 700;
    z-index: 1;
  }
}
</style>
