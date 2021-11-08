<template>
  <div>
    <drop-down>
      <template #drop-dow-toggle>
        <base-input />
      </template>
      <template #drop-dow-content>
        <div
          v-for="(item, i) in items"
          :key="`${item[propValue]}-${i}`"
          :class="[
            'multiselect-list-item',
            {
              'multiselect-list-item_selected': isSelected(item),
            },
          ]"
          @click="selectHandler(item)"
        >
          {{ item[propName] }}
        </div>
      </template>
    </drop-down>
  </div>
</template>

<script>
import Vue from "vue";
import DropDown from "../DropDown/DropDown";
import BaseInput from "../Input/BaseInput";
/*
@props
  items: [{
    id: 123,
    name: 'AAA'
  }]
 */
export default Vue.extend({
  name: "MultiSelect",
  props: {
    items: {
      type: Array,
      default: () => [],
    },
    selected: {
      type: Array,
      default: () => [],
    },
    propName: {
      type: String,
      default: "name",
    },
    propValue: {
      type: String,
      default: "name",
    },
  },
  components: {
    DropDown,
    BaseInput,
  },
  methods: {
    selectHandler(item) {
      this.$emit("selected", item);
    },
    isSelected(item) {
      return (
        this.selected.findIndex(
          (selected) => selected[this.propValue] === item[this.propValue]
        ) > -1
      );
    },
  },
});
</script>

<style scoped>
.multiselect-list-item {
  display: flex;
  padding: 8px;
}

.multiselect-list-item:hover {
  cursor: pointer;
  background-color: #f5f7fa;
}

.multiselect-list-item_selected {
  color: #216bff;
}
</style>