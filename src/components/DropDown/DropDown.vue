<template>
  <div v-click-outside="close">
    <div @click="toggle">
      <slot name="drop-down-toggle" />
    </div>

    <div v-show="opened || isOpen" class="drop-down-container">
      <slot name="drop-down-content" />
    </div>
  </div>
</template>

<script>
// import clickOutside from "@/utils/directives/clickOutside";

export default {
  name: "DropDown",
  
  props: {
    isOpen: {
      type: Boolean,
      default: false,
    },
  },
  updated() {
    this.opened = this.isOpen;
  },
  data() {
    return {
      opened: this.isOpen,
    };
  },
  methods: {
    toggle() {
      this.opened = !this.opened;
    },
    close() {
      this.opened = false;
    },
  },
};
</script>

<style scoped>
.drop-down-container {
  height: 150px;
  overflow-y: scroll;
}
</style>