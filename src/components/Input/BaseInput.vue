<template>
  <div :class="inputClasses">
    <input
      v-click-inside="focusHandler"
      class="base-input"
      :type="$attrs.type"
      :disabled="disabled"
      :autofocus="autofocus"
      :value="value"
      @blur="blurHandler"
      @input="inputHandler($event.target.value)"
    />

    <icon-clear v-show="showIconClear" @click="inputHandler('')" />

    <div v-show="showPlaceholder" :class="placeholderClasses">
      {{ $attrs.placeholder }}
    </div>
  </div>
</template>

<script>
// import IconClear from "@/components/Icons/IconClear";
// import clickInside from "@/utils/directives/clickInside";
// import BaseInputMixin from "@/components/Input/BaseInputMixin";
import IconClear from "../Icons/IconClear.vue";
import BaseInputMixin from "../Input/BaseInputMixin";

export default {
  name: "BaseInput",
  mixins: [BaseInputMixin],
  components: {
    IconClear,
  },  
  data() {
    return {
      focused: this.isActive,
    };
  },
  inheritAttrs: false,
  computed: {
    isActive() {
      return (this.autofocus && !this.disabled && this.focused) || this.value;
    },
    inputClasses() {
      return [
        "base-input-wrapper",
        {
          "base-input-wrapper_focused": this.isActive,
          "base-input-wrapper_disabled": this.disabled,
        },
      ];
    },
    showIconClear() {
      return this.value && this.value.length;
    },
    showPlaceholder() {
      return this.$attrs.placeholder && this.$attrs.placeholder.length;
    },
    placeholderClasses() {
      return [
        "base-input-placeholder",
        {
          "base-input-placeholder_lifted": this.isActive,
        },
      ];
    },
  },
  methods: {
    inputHandler(value) {
      this.$emit("input", value);
    },
    focusHandler() {
      this.focused = true;
    },
    blurHandler() {
      this.focused = false;
    },
  },
};
</script>

<style scoped>
.base-input-wrapper {
  position: relative;
  display: inline-flex;
  width: 100%;
  border-bottom: 2px solid #d4d9e2;
}

.base-input-wrapper_focused {
  border-bottom-color: #216bff;
}

.base-input {
  width: 100%;
  height: 30px;
  padding: 6px 16px 6px 0;
  box-sizing: border-box;
  margin: 0;
  border: 0;
  outline: 0;
  background-color: transparent;
}

.base-input-placeholder {
  position: absolute;
  display: flex;
  width: 100%;
  color: #8d95a5;
  white-space: nowrap;
  text-overflow: ellipsis;
  pointer-events: none;
  transform-origin: left top;
  transition: 300ms;
}

.base-input-placeholder_lifted {
  transform: translateY(-20px) scale(0.75);
}
</style>
