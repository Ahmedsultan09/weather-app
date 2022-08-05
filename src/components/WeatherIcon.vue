<template>
  <img :src="getImageUrl(this.icon)" alt="" />
</template>

<script>
export default {
  name: "WeatherIcon",
  props: ["icon"],
  data() {
    return {
      iconUrl: `./src/assets/weather-icons/${this.icon}.png`,
    };
  },
  methods: {
    getImageUrl(name) {
      return new URL(
        `../../src/assets/weather-icons/${name}.png`,
        import.meta.url
      ).href;
    },
  },
};
</script>

<style scoped></style>
