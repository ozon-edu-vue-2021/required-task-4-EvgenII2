<template>
  <div>
    <p class="form-label">{{ label }}</p>
    <input
      type="radio"
      :id="idFirst"
      value="true"
      @change="onChangeTrue"
      v-model="isTrue"
    />
    <label :for="idFirst">{{ innerTextFirst }}</label>
    <input
      type="radio"
      :id="idSecond"
      value="false"
      @change="onChangeFalse"
      v-model="isTrue"
    />
    <label :for="idSecond">{{ innerTextSecond }}</label>
  </div>
</template>

<script>
export default {
  props: {
    idFirst: String,
    idSecond: String,
    label: String,
    innerTextFirst: String,
    innerTextSecond: String,
    isTrueProps: { Boolean, default: false },
  },
  data() {
    return {
      isTrue: this.isTrueProps,
    };
  },
  methods: {
    onChangeTrue() {
      this.$emit("update", true);
    },
    onChangeFalse() {
      this.$emit("update", false);
    },
  },
};
</script>

<style scoped>
.form-label {
  padding: 0;
  margin: 0 0 20px;
  font-size: 18px;
}
</style>