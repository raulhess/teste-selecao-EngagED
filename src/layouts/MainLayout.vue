<template>
  <q-layout view="lhh Lpr lff" class="bg">
    <q-header class="bg-transparent text-primary text-center">
      <q-toolbar style="height: 130px">
        <div class="text-center full-width">
          <q-img src="logo.png" :style="logoStyle"></q-img>
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer class="text-center bg-secondary text-grey-9 q-pa-md bordered-top">
      Feito por Raul Hess para o desafio Front-End Vue.js EngagED
    </q-footer>
  </q-layout>
</template>

<script>
import { defineComponent, computed } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "MainLayout",

  setup() {
    const $q = useQuasar();

    const logoStyle = computed(() => {
      return $q.screen.gt.xs ? "width: 400px" : "width: 300px";
    });

    return {
      logoStyle,
    };
  },
});
</script>
