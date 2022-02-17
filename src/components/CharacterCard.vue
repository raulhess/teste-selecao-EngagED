<template>
  <div>
    <q-card square class="hoverable q-pa-none bordered" v-ripple:primary>
      <div :class="wrapperClasses">
        <q-img
          :class="imageClasses"
          :src="image"
          style="min-width: 174px"
        ></q-img>
        <q-card-section class="col">
          <div class="header-2 q-mb-xs">{{ species }}</div>
          <div class="header-1">{{ name }}</div>
          <div>{{ location }}</div>
        </q-card-section>
      </div>
    </q-card>
  </div>
</template>

<script>
import { computed, defineComponent } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "CharacterCard",

  props: {
    characterData: {
      type: Object,
      default() {
        return {};
      },
    },
  },

  setup(props) {
    const $q = useQuasar();

    const location = computed(() => {
      return props?.characterData?.location?.name ?? "";
    });

    const image = computed(() => {
      return props?.characterData?.image ?? "";
    });

    const name = computed(() => {
      return props?.characterData?.name ?? "";
    });

    const species = computed(() => {
      return props?.characterData?.species ?? "";
    });

    const imageClasses = computed(() => {
      let classes = ["col-auto"];
      classes.push($q.screen.gt.xs ? "bordered-right" : "bordered-bottom");
      return classes;
    });

    const wrapperClasses = computed(() => {
      let classes = [];
      classes.push($q.screen.gt.xs ? "row" : "column");
      return classes;
    });

    return {
      location,
      image,
      name,
      species,
      imageClasses,
      wrapperClasses,
    };
  },
});
</script>

<style>
</style>