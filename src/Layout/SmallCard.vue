<template>
  <div class="small-card sm:w-full lg:w-2/5 md:w-1/2">
    <div class="title">{{ title }}</div>
    <div class="status text-red-900">
      <span v-show="data !== null && type === 'visibility'"
        ><span>{{ data }}</span> Miles</span
      >
      <span v-show="data !== null && type === 'pressure'"
        ><span>{{ data }}</span> Mb</span
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "SmallCard",
  props: ["title", "data", "type"],
};
</script>

<style scoped>
.small-card {
  height: 35%;
  background-color: #1e213a;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.small-card .title {
  font-size: 16px;
  color: #88869d;
  padding: 10px;
}
.small-card .status {
  width: 100%;
  height: 100%;
  color: white;
  text-align: center;
  text-overflow: ellipsis;
}
.small-card .status span {
  font-size: 1.5rem;
  color: white;
  font-weight: bold;
}
</style>
