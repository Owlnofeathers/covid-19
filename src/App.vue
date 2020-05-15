<template>
  <div id="app">
    <div class="container mx-auto px-10 pb-10">
      <h1
        class="text-3xl text-gray-800 font-extrabold text-center my-5 md:mt-20 md:text-5xl"
      >
        COVID-19 Totals By US County
      </h1>
      <div class="md:text-center md:px-32">
        <vue-simple-suggest
          placeholder="Enter your county..."
          :list="countyCodes"
          display-attribute="name"
          :filter-by-query="true"
          @select="onSelect"
        ></vue-simple-suggest>
        <p v-if="error" class="text-red-700 text-sm">{{ error }}</p>
        <p v-if="stateData" class="mt-3 mb-5 text-sm">
          Last Updated: {{ new Date(stateData.lastUpdatedDate).toDateString() }}
        </p>
      </div>
      <div class="md:px-32 md:flex">
        <transition name="fade">
          <DisplayData
            v-if="stateData"
            :title="`${stateData.stateName} State`"
            :data="stateData"
            class="md:mr-10"
          ></DisplayData>
        </transition>
        <transition name="fade">
          <DisplayData
            v-if="countyData"
            :title="`${countyData.countyName} County, ${countyData.stateName}`"
            :data="countyData"
            class="mt-5 md:mt-0"
          ></DisplayData>
        </transition>
      </div>
      <Footer></Footer>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import DisplayData from "./components/DisplayData";
import VueSimpleSuggest from "vue-simple-suggest";
import "vue-simple-suggest/dist/styles.css";
import Footer from "./components/Footer"; // Using a css-loader

export default {
  name: "App",
  components: { Footer, DisplayData, VueSimpleSuggest },
  data() {
    return {
      stateData: null,
      countyData: null,
      countyCodes: [],
      chosenCounty: null,
      error: null
    };
  },
  created() {
    this.fetchCountyData();
    this.fetchStateData();
    this.fetchCountyCodes();
  },
  computed: {
    states() {
      // an array of state abbreviations and their FIPS code
      return [
        { name: "AL", id: "01" },
        { name: "AK", id: "02" },
        { name: "AZ", id: "04" },
        { name: "AR", id: "05" },
        { name: "CA", id: "06" },
        { name: "LA", id: "22" },
        { name: "KY", id: "21" },
        { name: "CO", id: "08" },
        { name: "CT", id: "09" },
        { name: "DE", id: "10" },
        { name: "DC", id: "11" },
        { name: "FL", id: "12" },
        { name: "GA", id: "13" },
        { name: "HI", id: "15" },
        { name: "ID", id: "16" },
        { name: "IL", id: "17" },
        { name: "IN", id: "18" },
        { name: "IA", id: "19" },
        { name: "KS", id: "20" },
        { name: "ME", id: "23" },
        { name: "MD", id: "24" },
        { name: "MA", id: "25" },
        { name: "MI", id: "26" },
        { name: "MN", id: "27" },
        { name: "MS", id: "28" },
        { name: "MO", id: "29" },
        { name: "MT", id: "30" },
        { name: "NE", id: "31" },
        { name: "NV", id: "32" },
        { name: "NH", id: "33" },
        { name: "NJ", id: "34" },
        { name: "NM", id: "35" },
        { name: "NY", id: "36" },
        { name: "NC", id: "37" },
        { name: "ND", id: "38" },
        { name: "OH", id: "39" },
        { name: "OK", id: "40" },
        { name: "OR", id: "41" },
        { name: "PA", id: "42" },
        { name: "RI", id: "44" },
        { name: "SC", id: "45" },
        { name: "SD", id: "46" },
        { name: "TN", id: "47" },
        { name: "TX", id: "48" },
        { name: "UT", id: "49" },
        { name: "VT", id: "50" },
        { name: "VA", id: "51" },
        { name: "WA", id: "53" },
        { name: "WV", id: "54" },
        { name: "WI", id: "55" },
        { name: "WY", id: "56" },
        { name: "PR", id: "72" }
      ];
    }
  },
  methods: {
    fetchCountyData(fipscode = "53011") {
      return axios
        .get(
          `https://data.covidactnow.org/latest/us/counties/${fipscode}.NO_INTERVENTION.json`
        )
        .catch(() => {
          this.error =
            "We either weren't able to get data for this county, or no cases have yet been confirmed.";
        })
        .then(response => {
          if (response && response.status === 200) {
            this.countyData = response.data;
            this.error = null;
          }
        });
    },
    fetchStateData(state = "WA") {
      return axios
        .get(
          `https://data.covidactnow.org/latest/us/states/${state}.NO_INTERVENTION.json`
        )
        .then(response => {
          this.stateData = response.data;
        });
    },
    fetchCountyCodes() {
      // grab a csv of counties and FIPS from census.gov and turn it into an array of objects
      return axios
        .get("https://api.census.gov/data/2010/dec/sf1?get=NAME&for=county:*")
        .then(async response => {
          // make the object response an array
          const array = Object.keys(response).map(k => response[k]);
          // flatten the array
          const flatArray = Array.from(array.flat());
          // make an array of objects with assigned keys
          flatArray.forEach(i => {
            this.countyCodes.push({ ["name"]: i[0], ["id"]: i[1] + i[2] });
          });
        });
    },
    onSelect(item) {
      this.countyData = null;
      this.stateData = null;
      this.chosenCounty = item.id;
      this.fetchCountyData(this.chosenCounty);
      const state = this.states.filter(
        // get state FIPS from first 2 numbers in county FIPS
        state => state.id === this.chosenCounty.substring(0, 2)
      );
      this.fetchStateData(state[0].name);
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
