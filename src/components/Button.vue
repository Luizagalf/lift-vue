<template>
  <div class="button">
    <button @click="action">{{ name }}</button>
    <div></div>
  </div>
</template>

<script lang="ts">
export default {
  name: "Button",
  props: {
    name: Number,
    active: Boolean,
    action: {
      type: Function,
      // eslint-disable-next-line @typescript-eslint/no-empty-function
      default: () => {}
    }
  }
};
</script>

<style scoped>
.button {
  height: 100px;
  display: flex;
  align-items: center;
}
</style>
