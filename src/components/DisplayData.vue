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
    <h1 class="text-3xl text-blue-600 font-bold mt-6">Cases</h1>

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

    <h1 class="text-3xl text-blue-600 font-bold mt-6">ICU Beds</h1>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current COVID Cases In ICU:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{
        data.actuals.icuBeds.currentUsageCovid.toLocaleString()
      }}</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current NON-COVID Cases In ICU:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{
        (
          data.actuals.icuBeds.currentUsageTotal -
          data.actuals.icuBeds.currentUsageCovid
        ).toLocaleString()
      }}</span>
    </p>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">Hospital Beds</h1>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current COVID Cases In Hospitals:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{
        data.actuals.hospitalBeds.currentUsageCovid.toLocaleString()
      }}</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Current NON-COVID Cases In Hospitals:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{
        (
          data.actuals.hospitalBeds.currentUsageTotal -
          data.actuals.hospitalBeds.currentUsageCovid
        ).toLocaleString()
      }}</span>
    </p>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">Vaccines</h1>
    <p>(Pfizer/ Moderna)</p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >First shots:</span
      >
      <span class="text-gray-800 text-2xl font-bold">{{
        data.actuals.vaccinationsInitiated.toLocaleString()
      }}</span>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Second shots:</span
      >
      <span class="text-gray-800 text-2xl font-bold mr-2">{{
        data.actuals.vaccinationsCompleted.toLocaleString()
      }}</span>
      <span>({{ percentVaccinated }}%)</span>
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
    percentCases() {
      return this.data.actuals.cases
        ? ((this.data.actuals.cases / this.data.population) * 100).toFixed(2)
        : 0;
    },
    percentDeaths() {
      return this.data.actuals.deaths
        ? ((this.data.actuals.deaths / this.data.population) * 100).toFixed(2)
        : 0;
    },
    percentVaccinated() {
      return this.data.actuals.vaccinationsCompleted
        ? (
            (this.data.actuals.vaccinationsCompleted / this.data.population) *
            100
          ).toFixed(2)
        : 0;
    }
  }
};
</script>

<style scoped></style>
