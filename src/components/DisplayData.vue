<template>
  <div>
    <h3 class="text-4xl text-gray-800 font-bold text-center">{{ title }}</h3>
    <p class="mt-5">
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Population:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{
        data.population.toLocaleString()
      }}</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current Cumulative Cases:</span
      >
      <span class="text-gray-800 text-2xl font-bold mr-2">{{
        data.actuals.cumulativeConfirmedCases.toLocaleString()
      }}</span>
      <span>({{ percentCases }}%)</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current Cumulative Deaths:</span
      >
      <span class="text-gray-800 text-2xl font-bold mr-2">{{
        data.actuals.cumulativeDeaths.toLocaleString()
      }}</span>
      <span>({{ percentDeaths }}%)</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current Cumulative Positive Tests:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{ positiveTests }}</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current Cumulative Negative Tests:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{ negativeTests }}</span>
    </p>
  </div>
</template>

<script>
export default {
  name: "DisplayData",
  props: {
    title: {
      required: true,
      type: String
    },
    data: {
      type: Object,
      required: true
    }
  },
  computed: {
    positiveTests() {
      return this.data.actuals.cumulativePositiveTests
        ? this.data.actuals.cumulativePositiveTests.toLocaleString()
        : "None Reported";
    },
    negativeTests() {
      return this.data.actuals.cumulativeNegativeTests
        ? this.data.actuals.cumulativeNegativeTests.toLocaleString()
        : "None Reported";
    },
    percentCases() {
      return this.data.actuals.cumulativeConfirmedCases
        ? (
            (this.data.actuals.cumulativeConfirmedCases /
              this.data.population) *
            100
          ).toFixed(3)
        : 0;
    },
    percentDeaths() {
      return this.data.actuals.cumulativeDeaths
        ? (
            (this.data.actuals.cumulativeDeaths / this.data.population) *
            100
          ).toFixed(3)
        : 0;
    }
  }
};
</script>

<style scoped></style>
