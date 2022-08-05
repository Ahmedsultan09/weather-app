<template>
  <ForecastCard class="forecast-day lg:w-1/6 md:w-1/3 sm:w-full">
    <div class="forecast-date">
      {{ dateConvert(allData.days[number].datetimeEpoch) }}
    </div>
    <div class="forecast-icon">
      <img :src="getImageUrl(allData.days[number].icon)" />
    </div>
    <div class="forecast-degree">
      <span class="deg-max" v-show="degreeType === 'C'"
        >{{ degConvert(allData.days[number].tempmax) }}°C</span
      >
      <span class="deg-max" v-show="degreeType === 'F'"
        >{{ allData.days[number].tempmax }}°F</span
      >
      <span class="deg-min" v-show="degreeType === 'C'"
        >{{ degConvert(allData.days[number].tempmin) }}°C</span
      >
      <span class="deg-min" v-show="degreeType === 'F'"
        >{{ allData.days[number].tempmin }}°F</span
      >
    </div>
  </ForecastCard>
</template>

<script>
import ForecastCard from "../Layout/ForecastCard.vue";
export default {
  name: "ForeCastDay",
  props: ["allData", "number", "degreeType"],
  components: [ForecastCard],
  methods: {
    degConvert(f) {
      let c;
      c = ((f - 32) * 5) / 9;
      return Math.ceil(c);
    },
    dateConvert(ePosh) {
      let myDate = new Date(ePosh * 1000).toDateString().split(" ");
      return `${myDate[0]}, ${myDate[2]} ${myDate[1]}`;
    },
    getImageUrl(name) {
      return new URL(
        `../../src/assets/weather-icons/${name}.png`,
        import.meta.url
      ).href;
    },
  },
};
</script>

<style scoped>
.forecast-day {
  height: 80%;
  padding: 10px 0;
  background-color: #1e213a;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.forecast-date {
  width: 100%;
  height: 20%;
  font-size: 18px;
  color: white;
  display: flex;
  justify-content: center;
}

.forecast-icon {
  width: 100%;
  height: 60%;
  display: flex;
  justify-content: center;
}

.forecast-degree {
  max-width: 100%;
  height: 20%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-direction: row;
  padding: 0 10px;
}

.forecast-degree .deg-max {
  color: white;
  font-size: 18px;
}
.forecast-degree .deg-min {
  color: #a09fb1;
  font-size: 14px;
}
.forecast-icon img {
  max-width: 100%;
  width: 100px;
  height: 100px;
}
</style>
