<template>
  <form>
    <label>Call List Size</label>
    <input
      type="text"
      id="call-list-size"
      name="call-list-size"
      v-bind:value="callListSize"
    />
    <input type="text" />
  </form>
</template>

<script>
export default {
  props: {
    callListSize: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped></style>
