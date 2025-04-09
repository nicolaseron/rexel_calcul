<template>
  <main class="m-5">
    <section id="pv">
      <h1 class="text-3xl underline font-bold">Calcul prix de vente</h1>
      <div class="mt-10 flex items-center gap-x-5">
        <input type="number" placeholder="Prix de achat" class="border px-2 py-1" @input="calcPVFn"
               v-model="calcPV.pa"/>
        <input type="number" placeholder="Marge" class="border px-2 py-1" @input="calcPVFn" v-model="calcPV.margin"/>
        <span>=</span>
        <p>{{ calcPV.result || '' }}</p>
        <span>€</span>
        <button @click="(e) => copyToClipboard(e, calcPV.result)" class="border p-2">Copier la valeur</button>
      </div>
    </section>
    <section id="pourcentage" class="my-20">
      <h1 class="text-3xl underline font-bold">Calcul pourcentage</h1>
      <div class="mt-10 flex items-center gap-x-5">
        <input type="number" placeholder="Prix de vente" class="border px-2 py-1" @input="calcPercentageFn"
               v-model="calcPercentage.pv"/>
        <select class="border px-2 py-1" v-model="calcPercentage.operator" @change="calcPercentageFn">
          <option value="+">Plus</option>
          <option value="-">Moins</option>
        </select>
        <input type="number" placeholder="Pourcentage" class="border px-2 py-1" @input="calcPercentageFn"
               v-model="calcPercentage.percentage"/>
        <span>=</span>
        <p>{{ calcPercentage.result || '' }}</p>
        <span>€</span>
        <button @click="(e) => copyToClipboard(e, calcPercentage.result)" class="border p-2">Copier la valeur</button>
      </div>
    </section>
    <section id="margin">
      <h1 class="text-3xl underline font-bold">Calcul marge</h1>
      <div class="mt-10 flex items-center gap-x-5">
        <input type="number" placeholder="Prix de achat" class="border px-2 py-1" @input="calcMarginFn"
               v-model="calcMargin.pa"/>
        <input type="number" placeholder="Prix de vente" class="border px-2 py-1" @input="calcMarginFn"
               v-model="calcMargin.pv"/>
        <span>=</span>
        <p>{{ calcMargin.result || '' }}</p>
        <span>%</span>
        <button @click="(e) => copyToClipboard(e, calcMargin.result)" class="border p-2">Copier la valeur</button>
      </div>
    </section>
    <button @click="resetFields" class="border p-2 block mx-auto mt-16 bg-red-600 text-white">Videz les champs</button>
  </main>
</template>

<script setup lang="ts">
import {ref} from "vue";

const calcPV = ref({
  pa: ref<number | null>(null),
  margin: ref<number | null>(null),
  result: ref<number | null>(null)
})

const calcPercentage = ref({
  pv: ref<number | null>(null),
  operator: ref<string>('+'),
  percentage: ref<number | null>(null),
  result: ref<number | null>(null)
})

const calcMargin = ref({
  pv: ref<number | null>(null),
  pa: ref<number | null>(null),
  result: ref<number | null>(null)
})

const calcPVFn = () => {
  if (calcPV.value.pa && calcPV.value.margin) {
    const m = (100 - calcPV.value.margin) / 100

    calcPV.value.result = (calcPV.value.pa / m).toFixed(4) as unknown as number
  }
}

const calcPercentageFn = () => {
  if (calcPercentage.value.pv && calcPercentage.value.percentage) {
    const p = calcPercentage.value.percentage / 100

    if (calcPercentage.value.operator === '+') {
      calcPercentage.value.result = (calcPercentage.value.pv + (calcPercentage.value.pv * p)).toFixed(4) as unknown as number
    } else {
      calcPercentage.value.result = (calcPercentage.value.pv - (calcPercentage.value.pv * p)).toFixed(4) as unknown as number
    }
  }
}

const calcMarginFn = () => {
  if (calcMargin.value.pv && calcMargin.value.pa) {
    const m = (1 - (calcMargin.value.pa / calcMargin.value.pv)) * 100

    calcMargin.value.result = m.toFixed(4) as unknown as number
  }
}

const resetFields = () => {
  calcPV.value.pa = null
  calcPV.value.margin = null
  calcPV.value.result = null

  calcPercentage.value.pv = null
  calcPercentage.value.percentage = null
  calcPercentage.value.result = null

  calcMargin.value.pv = null
  calcMargin.value.pa = null
  calcMargin.value.result = null
}

const copyToClipboard = (e: Event, value: number | null) => {
  if (!value) {
    alert('Aucune valeur à copier')
    return
  }
  navigator.clipboard.writeText(value.toString()).then(() => {
    (e.target as HTMLButtonElement).innerText = 'Valeur copiée !';
    (e.target as HTMLButtonElement).classList.add('bg-green-600', 'text-white')
    setTimeout(() => {
      (e.target as HTMLButtonElement).innerText = 'Copier la valeur';
      (e.target as HTMLButtonElement).classList.remove('bg-green-600', 'text-white')
    }, 2000)
  }).catch(err => {
    console.error('Erreur lors de la copie : ', err)
  })
}

</script>

<style scoped>

</style>
