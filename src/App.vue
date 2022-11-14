<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';
const calcAmount = ref(1000000);
const calcVolume = ref(50000);
const outcome = ref(0);
const outcomeMonthly = ref(0);
const outcomeMarketing = ref(0);
const stakedAmount = ref((1000000000 / 2 / 100) * 50);
const addAmount = (amount: number) => {
  calcAmount.value += amount;
};
const resetAmount = () => {
  calcAmount.value = 0;
};

const addVolume = (amount: number) => {
  calcVolume.value += amount;
};
const resetVolume = () => {
  calcVolume.value = 0;
};

const calcOutcome = ([_calcAmount, _calcVolume]) => {
  //  earning = volume * 0,01 * 0,33 * (amount of your staked tokens/total amount of staked tokens)
  outcome.value =
    (calcVolume.value * 0.01 * 0.33 * calcAmount.value) / stakedAmount.value;

  outcomeMonthly.value = outcome.value * 30;

  outcomeMarketing.value = calcVolume.value * 0.01 * 0.33 * 30;
};

watch([calcAmount, calcVolume], calcOutcome);

onMounted(() => {
  calcOutcome([calcAmount, calcVolume]);
});
</script>

<template>
  <div class="flex flex-col justify-start items-start">
    <p class="p-1 my-4 font-bold text-xl">
      Assuming {{ stakedAmount.toLocaleString(['en']) }} KATANA total staked
      amount
    </p>
    <span class="p-1">KATANA amount for staking</span>
    <div
      class="
        p-2
        bg-white
        border border-solid border-grey-500
        rounded
        flex flex-row
        items-center
        gap-2
        w-full
        justify-end
      "
    >
      <span class="p-3">{{ calcAmount.toLocaleString(['en']) }}</span>
      <button @click="addAmount(10000)" class="rounded bg-gray-200 p-2">
        +10000
      </button>
      <button @click="addAmount(100000)" class="rounded bg-gray-200 p-2">
        +100000
      </button>
      <button @click="addAmount(1000000)" class="rounded bg-gray-200 p-2">
        +1000000
      </button>
      <button @click="resetAmount()" class="rounded bg-gray-200 p-2">
        reset
      </button>
    </div>
    <span class="p-1">Daily trading volume in $</span>
    <div
      class="
        p-2
        bg-white
        border border-solid border-grey-500
        rounded
        flex flex-row
        items-center
        gap-2
        w-full
        justify-end
      "
    >
      <span class="p-3">${{ calcVolume.toLocaleString(['en']) }}</span>
      <button @click="addVolume(1000)" class="rounded bg-gray-200 p-2">
        +1000
      </button>
      <button @click="addVolume(10000)" class="rounded bg-gray-200 p-2">
        +10000
      </button>
      <button @click="addVolume(100000)" class="rounded bg-gray-200 p-2">
        +100000
      </button>
      <button @click="resetVolume()" class="rounded bg-gray-200 p-2">
        reset
      </button>
    </div>
    <div class="w-full flex flex-col align-center justify-center">
      <span class="p-2">
        Potential Daily Staking Rewards<br />
        <span class="text-xl">${{ outcome.toLocaleString(['en']) }}</span>
      </span>
      <span class="p-2">
        Potential Monthly Staking Rewards<br />
        <span class="text-xl"
          >${{ outcomeMonthly.toLocaleString(['en']) }}</span
        >
      </span>
      <span class="p-2">
        Potential Monthly backup rewards for staking and chart boost<br />
        <span class="text-xl"
          >${{ outcomeMarketing.toLocaleString(['en']) }}</span
        >
      </span>
    </div>
  </div>
</template>

<style scoped></style>
