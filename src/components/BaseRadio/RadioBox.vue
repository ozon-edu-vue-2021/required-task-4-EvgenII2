<template>
  <label class="wrapper flex items-center">
    <input
      type="radio"
      :checked="isChecked"
      :value="value"
      @change="$emit('change', $event.target.value)"
    />
    {{ label }}
  </label>
</template>

<script>
export default {
  model: {
    prop: "modelValue",
    event: "change",
  },
  props: {
    label: { type: String, default: "", required: true },
    modelValue: { default: "" },
    value: { type: String, default: undefined },
  },
  computed: {
    isChecked() {
      return this.modelValue == this.value;
    },
  },
};
</script>

<style scoped>
</style>