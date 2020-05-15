<template>
  <div id="app">
    <div class="container mx-auto px-10 pb-32">
      <img
        class="object-contain h-32 w-full mt-10"
        alt="Image of Washington State with Clark County Highlighted"
        src="./assets/washington_highlighting_clark_county.png"
      />
      <h1 class="text-5xl text-gray-600 font-extrabold text-center mb-5">
        COVID-19 By US County
      </h1>
      <div class="md:text-center md:px-32">
        <p class="text-left text-grey-600 font-bold">Type your county here:</p>
        <vue-simple-suggest
          v-model="chosenCounty"
          :list="countyCodes"
          display-attribute="name"
          value-attribute="code"
          :filter-by-query="true"
        >
          <!-- Filter by input text to only show the matching results -->
        </vue-simple-suggest>
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
    </div>
  </div>
</template>

<script>
import axios from "axios";
import DisplayData from "./components/DisplayData";
import VueSimpleSuggest from "vue-simple-suggest";
import "vue-simple-suggest/dist/styles.css"; // Using a css-loader

export default {
  name: "App",
  components: { DisplayData, VueSimpleSuggest },
  data() {
    return {
      stateData: null,
      countyData: null,
      countyCodes: [],
      stateCodes: null,
      chosenCounty: null
    };
  },
  async created() {
    await this.fetchCountyData();
    await this.fetchStateData();
    await this.fetchCountyCodes();
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
    },
    fetchCountyCodes() {
      return axios
        .get("https://api.census.gov/data/2010/dec/sf1?get=NAME&for=county:*")
        .then(async response => {
          // make the object response an array
          const array = Object.keys(response).map(k => response[k]);
          // flatten the array
          const flatArray = Array.from(array.flat());
          // make an array of objects with assigned keys
          flatArray.forEach(i => {
            this.countyCodes.push({ ["name"]: i[0], ["code"]: i[1] + i[2] });
          });
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
