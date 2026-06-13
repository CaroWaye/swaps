<template>
  <div id="app-container">
    <h1>Sustainable Swaps</h1>
    <button @click="move(1)" type="button">NEXT</button>
    <div class="banner-container">
      <div class="lh-banner">
        <div class="copy-container">
          <p>
          <span class="title">Swap: </span>
          <span class="copy">{{ left }}</span></p>
        </div>
      </div>
      <div class="rh-banner">
        <div class="copy-container">
          <p>
          <span class="title">For: </span>
          <span class="copy">{{ right }}</span></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import { swapData } from "@/data/swaps"

const index = ref(0);

const left = computed(() => getSwapDataForIndex(index.value).left);
const right = computed(() => getSwapDataForIndex(index.value).right);

function getSwapDataForIndex(swapIndex: number): Swap {
  const swapData: Swap | null = dataForIndex(swapIndex)
  if (!!swapData) {
    return swapData
  } else {
    index.value = 0
    return dataForIndex(0) as Swap
  }
}

function dataForIndex(index: number): Swap | null {
  return !!swapData[index] ? swapData[index] : null
}

function move(change: number) {
  if (swapData[index.value + change]) {
    index.value += change;
  } else {
    index.value = 0
  }
}

type Swap = { left: string; right: string }
</script>