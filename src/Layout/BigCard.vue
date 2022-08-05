<template>
  <div class="big-card sm:w-full lg:w-2/5 md:w-1/2">
    <div class="title">{{ title }}</div>
    <div class="status">
      <span class="not-av" v-show="data === null"> Not Aviliable </span>
      <span
        >{{ data }}
        <span v-show="data !== null && type === 'windspeed'">mph</span></span
      >
      <span v-show="type === 'humidity'">%</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "BigCard",
  props: ["title", "data", "type"],
};
</script>

<style scoped>
.big-card {
  height: 45%;
  background-color: #1e213a;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
}

.big-card .title {
  font-size: 18px;
  color: #88869d;
  padding: 20px;
}
.big-card .status {
  color: white;
}
.big-card .status span {
  font-size: 2rem;
  color: white;
  font-weight: bold;
}
.big-card .status .not-av {
  font-size: 22px;
  margin: 15px 0;
}
</style>
