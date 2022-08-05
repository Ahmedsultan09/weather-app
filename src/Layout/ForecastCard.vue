<template>
  <div class="forecast-day">
    <slots></slots>
  </div>
</template>

<script>
export default {
  name: "ForecastCard",
};
</script>

<style scoped>
.forecast-day {
  width: calc((100% - 100px) / 5);
  height: 80%;
  padding: 10px 0;
  background-color: #1e213a;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
