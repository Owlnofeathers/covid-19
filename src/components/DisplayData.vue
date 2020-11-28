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
        data.actuals.cases.toLocaleString()
      }}</span>
      <span>({{ percentCases }}%)</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current Cumulative Deaths:</span
      >
      <span class="text-gray-800 text-2xl font-bold mr-2">{{
        data.actuals.deaths.toLocaleString()
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
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
      >Current COVID Cases In ICU:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{ data.metrics.icuHeadroomDetails.currentIcuCovid }}</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
      >Current NON-COVID Cases In ICU:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{ data.metrics.icuHeadroomDetails.currentIcuNonCovid }}</span>
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
      return this.data.actuals.positiveTests
        ? this.data.actuals.positiveTests.toLocaleString()
        : "None Reported";
    },
    negativeTests() {
      return this.data.actuals.negativeTests
        ? this.data.actuals.negativeTests.toLocaleString()
        : "None Reported";
    },
    percentCases() {
      return this.data.actuals.cases
        ? ((this.data.actuals.cases / this.data.population) * 100).toFixed(3)
        : 0;
    },
    percentDeaths() {
      return this.data.actuals.deaths
        ? (
            (this.data.actuals.deaths / this.data.population) *
            100
          ).toFixed(3)
        : 0;
    }
  }
};
</script>

<style scoped></style>
