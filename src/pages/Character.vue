<template>
  <q-page>
    <div class="q-mx-lg q-pt-md q-pb-md">
      <div class="wrapper bg-white bordered">
        <toolbar-vue>{{ name }}</toolbar-vue>
        <div :class="wrapperClass">
          <div :class="colClass">
            <q-img :src="image" class="bordered" :style="imageStyle"></q-img>
            <template v-if="species">
              <div class="content-caption q-mt-md">species</div>
              <div>{{ species }}</div>
            </template>
            <template v-if="type">
              <div class="content-caption q-mt-md">type</div>
              <div>{{ type }}</div>
            </template>
            <template v-if="gender">
              <div class="content-caption q-mt-md">gender</div>
              <div>{{ gender }}</div>
            </template>
            <template v-if="status">
              <div class="content-caption q-mt-md">status</div>
              <div>{{ status }}</div>
            </template>
            <template v-if="origin">
              <div class="content-caption q-mt-md">origin</div>
              <div>{{ origin }}</div>
            </template>
            <template v-if="location">
              <div class="content-caption q-mt-md">location</div>
              <div>{{ location }}</div>
            </template>
          </div>
          <div class="col">
            <div>
              <div class="text-bold q-pa-md">EPISODE LIST</div>
              <q-separator></q-separator>
              <q-list separator>
                <q-item
                  clickable
                  v-for="episode of episodes"
                  :key="episode.id"
                  @click="goToEpisode(episode)"
                >
                  <q-item-section side>
                    {{ episode.episode }}
                  </q-item-section>
                  <q-item-section>
                    <q-item-label>{{ episode.name }}</q-item-label>
                  </q-item-section>
                </q-item>
              </q-list>
              <q-separator></q-separator>
            </div>
          </div>
        </div>
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
      charId: route?.params?.id ?? -1,
    });
    const { result, onResult } = useQuery(
      gql`
        query ($charId: ID!) {
          character(id: $charId) {
            id
            name
            status
            species
            type
            gender
            origin {
              name
            }
            location {
              name
            }
            image
            episode {
              id
              name
              episode
            }
            created
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

    const goToEpisode = (episode) => {
      router.push(`/episode/${episode.id ?? -1}`);
    };

    const colClass = computed(() => {
      let classes = ["col-auto", "q-pa-md"];
      classes.push($q.screen.gt.xs ? "bordered-right" : "bordered-bottom");
      return classes;
    });

    const imageStyle = computed(() => {
      return $q.screen.gt.xs ? "width: 174px" : "height: 300px";
    });

    const wrapperClass = computed(() => {
      return $q.screen.gt.xs ? "row" : "column";
    });

    const episodes = computed(() => {
      return result.value?.character?.episode ?? [];
    });

    const gender = computed(() => {
      return result.value?.character?.gender ?? "";
    });

    const image = computed(() => {
      return result.value?.character?.image ?? "";
    });

    const location = computed(() => {
      return result.value?.character?.location.name ?? "";
    });

    const name = computed(() => {
      return result.value?.character?.name ?? "";
    });

    const origin = computed(() => {
      return result.value?.character?.origin.name ?? "";
    });

    const species = computed(() => {
      return result.value?.character?.species ?? "";
    });

    const status = computed(() => {
      return result.value?.character?.status ?? "";
    });

    const type = computed(() => {
      return result.value?.character?.type ?? "";
    });

    return {
      colClass,
      episodes,
      gender,
      image,
      imageStyle,
      location,
      name,
      origin,
      species,
      status,
      type,
      wrapperClass,

      goToEpisode,
    };
  },

  components: {
    ToolbarVue,
  },
});
</script>

<style>
</style>