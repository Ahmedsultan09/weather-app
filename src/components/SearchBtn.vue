<template>
  <div class="search">
    <div id="search-text" @click="$emit('toggle')">
      <p>Search for places</p>
    </div>
    <i class="fa-solid fa-location-crosshairs search-icon"></i>
  </div>
</template>

<script>
export default {
  name: "SearchBtn",
  emits: ["toggle"],
  methods() {
    this.$emit("toggle");
  },
};
</script>

<style scoped>
.search {
  width: 100%;
  height: 15%;
  box-sizing: border-box;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 30px;
  align-items: center;
}

#search-text {
  font-size: 18px;
  color: white;
  background-color: #6e707a;
  padding: 0 20px;
  width: auto;
  height: 40px;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.search-icon {
  border-radius: 50%;
  background-color: #6e707a;
  color: white;
  font-size: 24px;
  width: 40px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
</style>
