<template>
  <div>
    <label v-bind:for="id" class="form-label">
      {{ label }}
    </label>
    <input
      v-bind:id="id"
      v-bind:placeholder="placeholder"
      v-bind:type="type"
      v-bind:max="type === 'date' ? getToday() : false"
      class="form-input"
      autofocus
      required
      @input="onInput"
      @blur="checkInput"
    />
    <p v-if="isShowError" class="input-error">
      {{ error }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    id: String,
    label: String,
    error: String,
    placeholder: String,
    isShowError: Boolean,
    type: { String, default: "text" },
  },
  methods: {
    checkInput() {
      this.$emit("blurHandler");
    },
    onInput(ev) {
      this.$emit("update", ev.target.value);
    },
    getToday: function () {
      const today = new Date();
      let dd = today.getDate();
      let mm = today.getMonth() + 1;
      const yyyy = today.getFullYear();
      if (dd < 10) {
        dd = "0" + dd;
      }
      if (mm < 10) {
        mm = "0" + mm;
      }
      return yyyy + "-" + mm + "-" + dd;
    },
  },
};
</script>

<style scoped>
.form-input {
  width: 100%;
  font-size: 16px;
  border: 0;
  outline: 0;
  background: transparent;
  border-bottom: 1px solid grey;
}
.form-label {
  padding: 0;
  margin: 0 0 20px;
  font-size: 18px;
}
.input-error {
  color: red;
}
</style>