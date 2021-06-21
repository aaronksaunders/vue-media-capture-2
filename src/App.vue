<template>
  <ion-app>
    <other />
  </ion-app>
</template>

<script lang="ts">
import { IonApp } from "@ionic/vue";
import Other from "@/views/Other.vue"
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: {
    IonApp,
    Other,
  },
  setup() {
    return {};
  },
});
</script>

<style scoped>
</style>