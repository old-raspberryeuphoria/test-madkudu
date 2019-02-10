<template>
  <main class="main-column">
    <div v-if="isLoading">Loading</div>
    <template v-else>
      <MasterTable
        :data="tableData"
        :settings="settings"
        @updateSettings="updateSettings"
        v-if="!currentView || currentView === 'overview'"
      />
      <hornsByContinents
        :data="antelopesData"
        v-else-if="currentView === 'hornsByContinents'"
      />
    </template>
  </main>
</template>

<script>
import MasterTable from '@/ui/components/MasterTable/MasterTable.vue';
import HornsByContinents from '@/ui/components/Graphs/HornsByContinents.vue';

const defaultSettings = {
  filterBy: {
    continents: ['asia'],
    hornsType: ['twisted'],
  },
  sortBy: {
    key: 'name',
    direction: 1,
  },
  excludeKeys: [
    'id',
    'picture',
  ],
};

export default {
  name: 'MainColumn',
  components: {
    MasterTable,
    HornsByContinents,
  },
  props: {
    isLoading: {
      type: Boolean,
      default: true,
    },
    antelopesData: {
      type: Array,
      default: null,
    },
  },
  data() {
    return {
      currentView: null,
      settings: { ...defaultSettings },
    };
  },
  created() {
    this.currentView = window.location.hash.substr(1, window.location.hash.length - 1);

    window.addEventListener('hashchange', () => {
      this.currentView = window.location.hash.substr(1, window.location.hash.length - 1);
    });
  },
  methods: {
    /**
     * Update data settings after a fired event
     * @param {Object} settings
     */
    updateSettings(settings) {
      this.settings = settings;
    },
    /**
     * Filter data by keys
     * @param {Array} data
     * @param {Object} keys
     * @returns {Array} the filtered data
     */
    filterData(data, { continents, hornsType }) {
      if (continents.length || hornsType.length) {
        return data.filter((row) => {
          let isValid = false;

          if (continents.length && continents.includes(row.continent.toLowerCase())) {
            isValid = true;
          }

          if (hornsType.length && hornsType.includes(row.horns.toLowerCase())) {
            isValid = true;
          }

          return isValid;
        });
      }

      return data;
    },
    /**
     * Sort data by a key and a direction
     * @param {Array} data
     * @param {string} key One of the data properties
     * @param {number} direction 1 for ASC order, -1 for DESC
     * @returns {Array} the sorted data
     */
    sortData(data, { key, direction }) {
      return data.sort((a, b) => {
        if (a[key] < b[key]) { return -direction; }
        if (a[key] > b[key]) { return direction; }
        return 0;
      });
    },
  },
  computed: {
    /**
     * Returns antelopesData after some manipulations
     */
    tableData() {
      const { settings } = this;
      let data = [...this.antelopesData];

      if (settings.sortBy) {
        data = this.sortData(data, settings.sortBy);
      }

      if (settings.filterBy) {
        // data = this.filterData(data, settings.filterBy);
      }

      return data;
    },
  },
};
</script>

<style scoped lang="scss" src="./MainColumn.scss"></style>
