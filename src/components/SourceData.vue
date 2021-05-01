<template>
  <div class="space-y-2">
    <h3 class="text-4xl text-gray-800 font-bold text-center">Sources</h3>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Cases:</span
      >
      <a :href="data.annotations.cases.sources[0].url" class="link">
        {{ data.annotations.cases.sources[0].name }}
      </a>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >ICU Beds:</span
      >
      <a :href="data.annotations.icuBeds.sources[0].url" class="link">
        {{ data.annotations.icuBeds.sources[0].name }}
      </a>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Hospital Beds:</span
      >
      <a :href="data.annotations.hospitalBeds.sources[0].url" class="link">
        {{ data.annotations.hospitalBeds.sources[0].name }}
      </a>
    </p>
    <p>
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl"
        >Vaccinations:</span
      >
      <a
        :href="data.annotations.vaccinationsCompleted.sources[0].url"
        class="link"
      >
        {{ data.annotations.vaccinationsCompleted.sources[0].name }}
      </a>
    </p>
  </div>
</template>

<script>
export default {
  name: "SourceData",

  props: {
    data: {
      type: Object,
      required: true
    }
  }
};
</script>
