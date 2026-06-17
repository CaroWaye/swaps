<template>
  <div id="app-container">
    <header>
      <a href="https://carodev.com/">Go back</a>
      <h1>Swaps</h1>
    </header>
    <div class="banner-container">
      <div class="lh-banner">
        <div class="copy-container">
          <p>
          <span class="title">Swap: </span>
          <span class="copy">{{ left }}</span></p>
        </div>
      </div>
      <button @click="handleClick" :class="['refresh-btn', { spinning }]" type="button" aria-label="Next swap">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="23 4 23 10 17 10"/>
          <path d="M20.49 15a9 9 0 1 1-2.12-9.36L23 10"/>
        </svg>
      </button>
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
const spinning = ref(false);

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

function handleClick() {
  move(1);
  spinning.value = true;
  setTimeout(() => { spinning.value = false; }, 500);
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