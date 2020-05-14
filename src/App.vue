<template>
  <div id="app">
    <div class="container mx-auto px-10">
      <img
        class="object-contain h-48 w-full mt-10"
        alt="Image of Washington State with Clark County Highlighted"
        src="./assets/washington_highlighting_clark_county.png"
      />
      <h1 class="text-5xl text-gray-600 font-extrabold text-center">
        COVID-19
      </h1>
      <DisplayData
        v-if="stateData"
        :title="`Current data for ${stateData.stateName} State`"
        :data="stateData"
        class="my-10"
      ></DisplayData>
      <DisplayData
        v-if="countyData"
        :title="
          `Current data for ${countyData.countyName} County, ${countyData.stateName}`
        "
        :data="countyData"
        class="my-10"
      ></DisplayData>
      {{ countyData }}
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
