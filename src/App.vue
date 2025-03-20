<template>
  <main class="m-5">
    <section id="margin">
      <h1 class="text-3xl underline font-bold">Calcul marge</h1>
      <div class="mt-10 flex items-center gap-x-5">
        <input type="number" placeholder="Prix de vente" class="border px-2 py-1" @input="calcMargin" v-model="pv"/>
        <input type="number" placeholder="Marge" class="border px-2 py-1" @input="calcMargin" v-model="margin"/>
        <span>=</span>
        <p>{{ resultM || '' }}</p>
      </div>
    </section>
    <section id="pourcentage" class="mt-20">
      <h1 class="text-3xl underline font-bold">Calcul pourcentage</h1>
      <div class="mt-10 flex items-center gap-x-5">
        <input type="number" placeholder="Prix de vente" class="border px-2 py-1" @input="calcPercentage" v-model="pvp"/>
        <select class="border px-2 py-1" v-model="operator" @change="calcPercentage">
          <option value="+">Plus</option>
          <option value="-">Moins</option>
        </select>
        <input type="number" placeholder="Pourcentage" class="border px-2 py-1" @input="calcPercentage" v-model="percentage"/>
        <span>=</span>
        <p>{{ resultP || '' }}</p>
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
import {ref} from "vue";

const pv = ref<number | null>(null)
const margin = ref<number | null>(null)
const resultM = ref<number | null>(null)
const pvp = ref<number | null>(null)
const operator = ref<string>('+')
const percentage = ref<number | null>(null)
const resultP = ref<number | null>(null)

const calcMargin = () => {
  if (pv.value && margin.value) {
    const m = (100 - margin.value) / 100

    resultM.value = (pv.value / m).toFixed(4) as unknown as number
  }
}

const calcPercentage = () => {
  if (pvp.value && percentage.value) {
    const p = percentage.value / 100

    if (operator.value === '+') {
      resultP.value = (pvp.value + (pvp.value * p)).toFixed(4) as unknown as number
    } else {
      resultP.value = (pvp.value - (pvp.value * p)).toFixed(4) as unknown as number
    }
  }
}

</script>

<style scoped>

</style>
