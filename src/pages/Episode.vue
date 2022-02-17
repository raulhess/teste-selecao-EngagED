<template>
  <q-page>
    <div class="q-mx-lg q-pt-md q-pb-md">
      <div class="wrapper bg-white bordered">
        <toolbar-vue>
          <template v-if="$q.screen.gt.xs">{{ episode }} - {{ name }}</template>
        </toolbar-vue>
        <div class="bordered-bottom q-pa-md">
          <template v-if="$q.screen.lt.sm">
            <div class="content-caption">Name</div>
            <div class="q-mb-md">{{ episode }} - {{ name }}</div>
          </template>
          <template v-if="airDate">
            <div class="content-caption">Air Date</div>
            <div>{{ airDate }}</div>
          </template>
        </div>
        <div class="text-bold q-pa-md">CHARACTER LIST</div>
        <q-separator></q-separator>
        <q-list separator>
          <q-item
            clickable
            v-for="character of characters"
            :key="character.id"
            @click="goToCharacter(character)"
          >
            <q-item-section side>
              <q-avatar size="48px" square>
                <img :src="character.image" class="bordered" />
              </q-avatar>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ character.name }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
        <q-separator></q-separator>
      </div>
    </div>
  </q-page>
</template>

<script>
import { computed, defineComponent, onMounted, reactive } from "vue";
import { useQuery } from "@vue/apollo-composable";
import { useRoute, useRouter } from "vue-router";
import { useQuasar } from "quasar";
import gql from "graphql-tag";

import ToolbarVue from "src/components/Toolbar.vue";

export default defineComponent({
  name: "Character",

  setup() {
    const $q = useQuasar();
    const route = useRoute();
    const router = useRouter();

    const variables = reactive({
      episodeId: route?.params?.id ?? -1,
    });
    const { result, onResult } = useQuery(
      gql`
        query ($episodeId: ID!) {
          episode(id: $episodeId) {
            id
            name
            air_date
            episode
            characters {
              id
              image
              name
            }
          }
        }
      `,
      variables
    );

    onMounted(() => {
      $q.loading.show();
      setTimeout(() => {
        $q.loading.hide();
      }, 1000);
    });

    onResult(() => {
      $q.loading.hide();
    });

    const goToCharacter = (character) => {
      router.push(`/character/${character.id ?? -1}`);
    };

    const airDate = computed(() => {
      return result.value?.episode?.air_date ?? "";
    });

    const characters = computed(() => {
      return result.value?.episode?.characters ?? [];
    });

    const episode = computed(() => {
      return result.value?.episode?.episode ?? "";
    });

    const name = computed(() => {
      return result.value?.episode?.name ?? "";
    });

    return {
      airDate,
      characters,
      episode,
      name,

      goToCharacter,
    };
  },

  components: {
    ToolbarVue,
  },
});
</script>

<style>
</style>