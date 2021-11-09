<template>
  <div>
    <drop-down :isOpen="isOpenDrop">
      <template #drop-down-toggle @selectHandler="toggle">
        <base-input v-model="selected" />
      </template>
      <template #drop-down-content>
        <div
          v-for="item in items"
          :key="`${item.id}`"
          :class="'select-list-item'"
          @click="selectHandler(item.nationality)"
        >
          {{ item.nationality }}
        </div>
      </template>
    </drop-down>
  </div>
</template>

<script>
import Vue from "vue";
import DropDown from "../DropDown/DropDown";
import BaseInput from "../Input/BaseInput";

export default Vue.extend({
  name: "Select",
  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      selected: "",
      isOpenDrop: false,
    };
  },
  components: {
    DropDown,
    BaseInput,
  },
  methods: {
    selectHandler(nationality) {
      this.isOpenDrop = !this.isOpenDrop;
      this.selected = nationality;
      this.$emit("selected", nationality);
    },
  },
});
</script>

<style scoped>
.select-list-item {
  display: flex;
  padding: 8px;
}

.select-list-item:hover {
  cursor: pointer;
  background-color: #f5f7fa;
}

.select-list-item_selected {
  color: #216bff;
}
</style>