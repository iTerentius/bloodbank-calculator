<template>
  <input
    class="form-control"
    type="text"
    name="cInput"
    v-model="displayValue"
    @blur="isInputActive = false"
    @focus="isInputActive = true"
  />
</template>

<script>
export default {
  name: "CurrencyInput",
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
          return (
            "$" +
            this.modelValue
              .toFixed(2)
              // .replace(/^\d+\.\d{0,2}$/g, "$1,")
              .replace(/(\d)(?=(\d{3})+(?:\.\d+)?$)/g, "$1,")
          );
        }
      },
      set: function (modifiedValue) {
        // Recalculate value after ignoring "$" and "," in user input
        let newValue = parseFloat(modifiedValue);
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
