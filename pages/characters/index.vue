<template>
  <section class="docs-container container">
    <h1>Creating Characters</h1>
    <div class="docs-toc">
      <ul>
        <li v-for="section in charSections" :key="section.slug">
          <nuxt-link tag="a" :to="`/sections/${section.slug}`">
            {{ section.name }}
          </nuxt-link>
        </li>
        <li>
          Races
          <ul>
            <li>
              <nuxt-link tag="a" to="/races/tiefling"> Tiefling </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/dragonborn"> Dragonborn </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/dwarf"> Dwarf </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/elf"> Elf </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/gnome"> Gnome </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/half-elf"> Half-Elf </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/half-orc"> Half-Orc </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/halfling"> Halfling </nuxt-link>
            </li>
            <li>
              <nuxt-link tag="a" to="/races/human"> Human </nuxt-link>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import { useMainStore } from '~/store';
Array.prototype.groupBy = function (prop) {
  return this.reduce(function (groups, item) {
    const val = item[prop];
    groups[val] = groups[val] || [];
    groups[val].push(item);
    return groups;
  }, {});
};

export default {
  setup() {
    const store = useMainStore();
    return { store };
  },
  computed: {
    sectionGroups: function () {
      let groupedSections = this.store.allSections.groupBy('parent');
      return groupedSections;
    },
    charSections: function () {
      if (this.sectionGroups.hasOwnProperty('Characters')) {
        let results = this.sectionGroups['Characters'].concat(
          this.sectionGroups['Character Advancement']
        );
        return results.sort(function (a, b) {
          if (a.slug < b.slug) {
            return -1;
          } else if (a.slug > b.slug) {
            return 1;
          } else {
            return 0;
          }
        });
      } else {
        return [];
      }
    },
  },
};
</script>

<style></style>
