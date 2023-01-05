<template>
  <div class="button">
    <button
      @click="setActiveBtnState(data.id)"
      :style="{
        backgroundColor: data.active ? `#806491` : `#f6e8f2`,
        color: data.active ? `#f6e8f2` : `#00457e`
      }"
    >
      {{ data.name }}
    </button>
  </div>
</template>

<script lang="ts">
import { BtnType } from "@/types/BtnType";
import { PropType } from "vue";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Button",
  props: {
    data: { type: Object as PropType<BtnType>, required: true },
    setActiveBtnState: { type: Function, required: true }
  }
});
</script>

<style lang="scss" src="./button.scss" scoped></style>
