<template>
  <div
    class="right lg:w-2/3 md:w-full sm:w-full lg:h-4/5 md:min-h-full sm:min-h-full"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "RightSide",
};
</script>

<style scoped>
.right {
  background: #100e1d;
}
</style>
