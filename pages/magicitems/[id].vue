<template>
  <section class="docs-container container">
    <p v-if="loading">Loading...</p>
    <div v-else>
      <h1 class="inline">
        {{ item.name }}
      </h1>
      <p>
        <em
          >{{ item.type }}, {{ item.rarity }}
          <span v-show="item.requires_attunement"
            >({{ item.requires_attunement }})</span
          ></em
        >
        <source-tag
          v-show="item.document__slug"
          :title="item.document__title"
          :text="item.document__slug"
        />
      </p>
      <md-viewer :text="item.desc" />
      <p class="text-sm italic">
        Source:
        <a target="NONE" :href="item.document__url">
          {{ item.document__title }}
          <Icon name="heroicons:arrow-top-right-on-square-20-solid" />
        </a>
      </p>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
import SourceTag from '~/components/SourceTag.vue';
import MdViewer from '~/components/MdViewer.vue';

export default {
  components: {
    MdViewer,
    SourceTag,
  },
  data() {
    return {
      item: [],
      loading: true,
    };
  },
  computed: {
    nextSpellId: function () {
      return this.item.id + 1;
    },
    prevSpellId: function () {
      return this.item.id - 1;
    },
  },
  mounted() {
    return axios
      .get(
        `${useRuntimeConfig().public.apiUrl}/magicitems/${
          this.$route.params.id
        }`
      ) //you will need to enable CORS to make this work
      .then((response) => {
        this.item = response.data;
        this.loading = false;
      });
  },
};
</script>

<style scoped>
label {
  font-weight: bold;
}

.inline {
  display: inline-block;
}
</style>
