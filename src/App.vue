<template>
  <div id="app">
    <div class="container mx-auto px-10 pb-32">
      <img
        class="object-contain h-32 w-full mt-10"
        alt="Image of Washington State with Clark County Highlighted"
        src="./assets/washington_highlighting_clark_county.png"
      />
      <h1 class="text-5xl text-gray-600 font-extrabold text-center">
        Clark County COVID-19
      </h1>
      <div class="md:text-center md:px-32">
        <p class="mb-3">
          This site was created and is maintained by
          <a href="https://adambailey.io/" class="link">Adam Bailey</a>
          for an easy place to check the covid-19 totals in Clark County.
        </p>
        <p>
          The data is aggregated by
          <a href="https://covidactnow.org/" class="link">CovidActNow.org</a>
          and pulled in via their
          <a
            href="https://github.com/covid-projections/covid-data-model/blob/master/api/README.V1.md"
            class="link"
            >Official API</a
          >.
        </p>
        <p class="text-gray-600">
          "Covid Act Now is a multidisciplinary team of technologists,
          epidemiologists, health experts, and public policy leaders working to
          provide disease intelligence and data analysis on COVID in the U.S"
        </p>
        <p v-if="stateData" class="mt-3 mb-5 text-sm">
          Last Updated: {{ new Date(stateData.lastUpdatedDate).toDateString() }}
        </p>
      </div>
      <div class="md:px-32 md:flex">
        <DisplayData
          v-if="stateData"
          :title="`${stateData.stateName} State`"
          :data="stateData"
          class="md:mr-10"
        ></DisplayData>
        <DisplayData
          v-if="countyData"
          :title="`${countyData.countyName} County, ${countyData.stateName}`"
          :data="countyData"
          class="mt-5 md:mt-0"
        ></DisplayData>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import DisplayData from "./components/DisplayData";

export default {
  name: "App",
  components: { DisplayData },
  data() {
    return {
      stateData: null,
      countyData: null
    };
  },
  created() {
    this.fetchCountyData();
    this.fetchStateData();
  },
  methods: {
    fetchCountyData() {
      return axios
        .get(
          "https://data.covidactnow.org/latest/us/counties/53011.NO_INTERVENTION.json"
        )
        .then(response => {
          this.countyData = response.data;
        });
    },
    fetchStateData() {
      return axios
        .get(
          "https://data.covidactnow.org/latest/us/states/WA.NO_INTERVENTION.json"
        )
        .then(response => {
          this.stateData = response.data;
        });
    }
  }
};
</script>
<style>
.link {
  color: blue;
}
.link:hover {
  text-decoration: underline;
}
</style>
