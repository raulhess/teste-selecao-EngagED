<template>
  <q-page>
    <div class="q-mx-lg q-pt-md">
      <div class="wrapper">
        <search-vue
          v-model="variables.name"
          placeholder="Pesquisar"
          @update:model-value="onInput"
        ></search-vue>
      </div>
    </div>
    <div class="q-mt-md q-mx-lg">
      <div class="wrapper q-gutter-y-md">
        <div class="row q-col-gutter-md">
          <character-card-vue
            class="col-12 col-sm-6"
            style="min-width: 200px"
            v-for="character of characters"
            :key="character.id"
            :characterData="character"
            @click="goToCharacter(character)"
          ></character-card-vue>
        </div>
        <div
          v-if="maxPages > 0"
          class="flex flex-center q-mb-md q-pa-sm bg-white bordered"
        >
          <q-pagination
            v-model="variables.page"
            :max="maxPages"
            :max-pages="$q.screen.gt.xs ? 10 : 7"
            unelevated
            color="grey-10"
            active-color="primary"
            active-text-color="white"
            direction-links
          >
          </q-pagination>
        </div>
        <div
          v-else-if="variables.name"
          class="flex flex-center q-mb-md q-pa-sm bg-white bordered"
        >
          <div class="text-center text-bold">
            Não foi possível encontrar nenhum resultado para a busca '{{
              variables.name
            }}'.
            <div>
              <q-icon
                name="las la-sad-cry"
                color="grey-10"
                size="100px"
              ></q-icon>
            </div>
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted, reactive, watch } from "vue";
import { useRouter } from "vue-router";
import { useQuery } from "@vue/apollo-composable";
import { useQuasar } from "quasar";
import gql from "graphql-tag";

import CharacterCardVue from "src/components/CharacterCard.vue";
import SearchVue from "src/components/Search.vue";

export default defineComponent({
  name: "PageIndex",

  components: {
    CharacterCardVue,
    SearchVue,
  },

  setup() {
    const $q = useQuasar();
    const router = useRouter();

    const maxPages = ref(0);
    const characters = ref([]);

    const variables = reactive({
      name: "",
      page: 1,
    });
    const { loading, onResult, restart } = useQuery(
      gql`
        query ($name: String!, $page: Int!) {
          characters(page: $page, filter: { name: $name }) {
            info {
              count
              pages
              next
              prev
            }
            results {
              id
              name
              image
              species
              location {
                id
                name
              }
            }
          }
        }
      `,
      variables
    );

    onMounted(() => {
      $q.loading.show();
      setTimeout(() => {
        restart();
        $q.loading.hide();
      }, 1000);
    });

    onResult(({ data }) => {
      console.log(data);
      if (loading.value) return;
      if (data) {
        characters.value = data?.characters?.results;
        maxPages.value = data?.characters?.info?.pages;
      } else {
        characters.value = [];
        maxPages.value = 0;
      }
    });

    watch(loading, (value) => {
      if (value) {
        $q.loading.show();
      } else {
        $q.loading.hide();
      }
    });

    const onInput = () => {
      variables.page = 1;
    };

    const goToCharacter = (character) => {
      router.push(`/character/${character.id ?? -1}`);
    };

    return {
      goToCharacter,
      onInput,

      characters,
      maxPages,
      variables,
    };
  },
});
</script>
