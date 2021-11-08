<template>
  <div class="chip">
    {{ item[propName] }}
    <icon-clear class="chip-clear-icon" @click="$emit('remove', item)" />
  </div>
</template>

<script>
import IconClear from "../Icons/IconClear.vue";

/*
@props
  item: {
    id: 123,
    name: 'AAA'
  }
 */
export default {
  name: "Chip",
  props: {
    item: {
      type: Object,
      default: () => ({}),
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
    IconClear,
  },
};
</script>

<style scoped>
.chip {
  display: inline-flex;
  align-items: center;
  padding: 4px 8px;
  box-sizing: border-box;
  color: #216bff;
  border: 1px solid #216bff;
  border-radius: 40px;
}

.chip-clear-icon {
  fill: #216bff;
  width: 16px;
  margin-left: 8px;
  cursor: pointer;
}
</style>