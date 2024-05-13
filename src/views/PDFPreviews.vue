g
<script setup lang="ts">
import { ref } from 'vue'
import PDFPage from '@/components/PDFPage.vue'
import { PageOrientation } from '@/types/PageOrientation'
import { v4 as uuidv4 } from 'uuid'
import ToggleButton from '@/components/ToggleButton.vue'

const previews = ref([
  {
    id: uuidv4(),
    orientation: 'portrait' as PageOrientation,
    offer: {
      id: 123,
      price: '3 för 2',
      image:
        'https://assets.icanet.se/$y_h,$x_w,$pi_current:public_id/c_fit,w_$x,h_$y,u_7340005403332,x_$y_mul_-0.3,y_$x_mul_-0.3/c_fit,w_$x,h_$y,u_7340005403516,x_$y_mul_-0.3,y_$x_mul_0.6,g_south_east/t_offer_square_highrez_v1/7340005403554.webp'
    }
  },
  {
    id: uuidv4(),
    orientation: 'landscape' as PageOrientation,
    offer: {
      id: 456,
      price: '35:-',
      image:
        'https://assets.icanet.se/$y_h,$x_w,$pi_current:public_id/c_fit,w_$x,h_$y,u_1220000450028,x_$y_mul_-0.3,y_$x_mul_0.6,g_south_east/t_offer_square_highrez_v1/1220000450035.webp'
    }
  },
  {
    id: uuidv4(),
    orientation: 'portrait' as PageOrientation,
    offer: {
      id: 789,
      price: '129:-',
      image: 'https://assets.icanet.se/t_offer_square_highrez_v1/2319117300001.webp'
    }
  }
])

const pageSize = ref('A4')

const onRemove = (id: string) => {
  previews.value = previews.value.filter((p) => p.id !== id)
}

const onDuplicate = (id: string) => {
  const index = previews.value.findIndex((p) => p.id === id)

  if (index !== -1) {
    const newItem = {
      ...previews.value[index],
      id: uuidv4(),
      offer: {
        ...previews.value[index].offer
      }
    }

    previews.value.splice(index + 1, 0, newItem)
  }
}

const onChangeOrientation = (orientation: PageOrientation) => {
  previews.value = previews.value.map((preview) => ({
    ...preview,
    orientation: orientation
  }))
}
</script>

<template>
  <header>
    <h1>Anpassa utskrift</h1>
    <div class="orientation-buttons">
      <button @click="onChangeOrientation('portrait')">Alla sidor stående</button>
      <button @click="onChangeOrientation('landscape')">Alla sidor liggande</button>
    </div>
    <ToggleButton
      leftToggle="A4"
      rightToggle="A3"
      :activeToggle="pageSize"
      @onToggle="pageSize = $event"
    />
  </header>

  <div class="pdf-previews">
    <PDFPage
      v-for="preview in previews"
      :key="`${preview.id}-${preview.orientation}`"
      :size="pageSize"
      :orientation="preview.orientation"
      :offer="preview.offer"
      @on-duplicate="onDuplicate(preview.id)"
      @on-remove="onRemove(preview.id)"
    />
  </div>
</template>

<style scoped>
@media print {
  .pdf-previews {
    display: block;
  }

  header {
    display: none;
  }
}

@media screen {
  .pdf-previews {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    place-items: center;
    padding: 1rem 0;
  }

  header {
    display: grid;
    place-items: center;
    gap: 0.5rem;
  }
}

header {
  h1 {
    font-size: 3rem;
    font-weight: 600;
  }

  .orientation-buttons {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.5rem;
  }
}

.pdf-previews {
  margin: 0;
}

button {
  padding: 0.5rem 1rem;
  font-size: small;
  color: white;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  background-color: #325b9a;
}
</style>
