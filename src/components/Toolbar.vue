<template>
  <q-toolbar class="bordered-bottom toolbar-title justify-center ellipsis">
    <div class="absolute" style="left: 8px">
      <q-btn icon="las la-arrow-left" flat @click="$router.go(-1)">
        <q-tooltip class="bg-black" style="border-radius: 0px; font-size: 14px">
          Go back
        </q-tooltip>
      </q-btn>
    </div>

    <slot> </slot>

    <div class="absolute" style="right: 8px">
      <q-btn icon-right="las la-home" flat @click="$router.push('/')">
        <q-tooltip class="bg-black" style="border-radius: 0px; font-size: 14px">
          Home
        </q-tooltip>
      </q-btn>
    </div>
  </q-toolbar>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "Toolbar",
});
</script>

<style>
</style>