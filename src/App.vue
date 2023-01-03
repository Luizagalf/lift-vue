<template>
  <Lift />
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Lift from "@/components/Lift.vue";

@Options({
  components: {
    Lift
  }
})
export default class HomeView extends Vue {}
</script>

<style></style>
