<template>
  <div class="row bg-white q-px-md bordered">
    <q-input
      v-model="search"
      dense
      class="full-width"
      borderless
      :debounce="500"
      :placeholder="placeholder"
      @update:model-value="$emit('update:model-value', search)"
    >
      <template v-slot:prepend>
        <q-icon color="primary" name="las la-search" class="q-mr-sm"></q-icon>
      </template>
    </q-input>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "Search",

  props: {
    modelValue: {
      type: String,
      default: "",
    },

    placeholder: {
      type: String,
      default: "",
    },
  },

  emits: ["update:model-value"],

  setup(props) {
    const search = ref(props.modelValue);

    return {
      search,
    };
  },
});
</script>

<style>
</style>