<template>
  <div>
    <h3 class="text-2xl text-gray-800 font-bold text-center">{{ title }}</h3>
    <p class="text-xl mt-10">
      <span class="text-gray-600 font-bold mr-2">Population:</span>
      <span class="text-gray-800 font-bold">{{
        data.population.toLocaleString()
      }}</span>
    </p>
    <p class="text-xl">
      <span class="text-gray-600 font-bold mr-2"
        >Current Cumulative Cases:</span
      >
      <span class="text-gray-800 font-bold">{{
        data.actuals.cumulativeConfirmedCases.toLocaleString()
      }}</span>
    </p>
    <p class="text-xl">
      <span class="text-gray-600 font-bold mr-2"
        >Current Cumulative Deaths:</span
      >
      <span class="text-gray-800 font-bold">{{
        data.actuals.cumulativeDeaths.toLocaleString()
      }}</span>
    </p>
    <p class="text-xl">
      <span class="text-gray-600 font-bold mr-2"
        >Current Cumulative Positive Tests:</span
      >
      <span class="text-gray-800 font-bold">{{ positiveTests }}</span>
    </p>
    <p class="text-xl">
      <span class="text-gray-600 font-bold mr-2"
        >Current Cumulative Negative Tests:</span
      >
      <span class="text-gray-800 font-bold">{{ negativeTests }}</span>
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
    }
  }
};
</script>

<style scoped></style>
