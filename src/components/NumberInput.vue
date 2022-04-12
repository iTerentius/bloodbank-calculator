<template>
  <input
    class="form-control"
    type="text"
    name="nInput"
    v-model="displayValue"
    @blur="isInputActive = false"
    @focus="isInputActive = true"
  />
</template>

<script>
export default {
  name: "NumberInput",
  props: ["modelValue"],
  emits: ["update:modelValue"],
  data() {
    return {
      isInputActive: false,
      val: this.modelValue,
      // value: 0,
    };
  },
  computed: {
    displayValue: {
      get: function () {
        if (this.isInputActive) {
          //Cursor is inside the input field. unformat display value for user
          return this.modelValue.toString();
        } else {
          // User is not modifying now. Format display value for user interface
          //const regex = "(\d{1,3}(?=(\d{3})+(?!\d))/g";
          return this.modelValue
            .toFixed(0)
            .replaceAll(/\d{1,3}(?=(\d{3})+(?!\d))/g, "$&,");
        }
      },
      set: function (modifiedValue) {
        // Recalculate value after ignoring "$" and "," in user input
        let newValue = parseFloat(modifiedValue.replace(/[^\d]/g, ""));
        // Ensure that it is not NaN
        if (isNaN(newValue)) {
          newValue = 0;
        }
        // Note: we cannot set this.value as it is a "prop". It needs to be passed to parent component
        // $emit the event so that parent component gets it
        this.$emit("update:modelValue", newValue);
      },
    },
  },
};
</script>
