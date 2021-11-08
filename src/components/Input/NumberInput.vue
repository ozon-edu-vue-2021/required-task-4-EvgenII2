<template>
  <base-input
    type="number"
    v-bind="{ ...$props, ...$attrs }"
    v-model="numberValue"
  />
</template>

<script>
import BaseInputMixin from "./BaseInputMixin";
import BaseInput from "./BaseInput";

export default {
  name: "NumberInput",
  components: { BaseInput },
  mixins: [BaseInputMixin],
  props: {
    value: {
      type: [Number, String],
      default: 0,
    },
    min: {
      type: Number,
      default: 0,
    },
    max: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    numberValue: {
      get() {
        return this.value ? this.value.toString() : "";
      },
      set(newValue) {
        this.$emit("input", this.parseNumber(newValue));
      },
    },
  },
  methods: {
    parseNumber(value) {
      let result = value === "" || isNaN(value) ? this.min : parseInt(value);

      if (this.isOutOfMin(result)) {
        result = this.min;
      }
      if (this.isOutOfMax(result)) {
        result = this.max;
      }

      return result;
    },
    isOutOfMin(value) {
      return value < this.min;
    },
    isOutOfMax(value) {
      return value > this.max;
    },
  },
};
</script>