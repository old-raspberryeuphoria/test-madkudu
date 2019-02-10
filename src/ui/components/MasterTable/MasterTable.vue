<template>
  <div>
    <h2>Overview</h2>
    <table class="master-table">
      <thead>
        <tr>
          <th v-for="(col, i) in schema" :key="i">
            <div class="th-content" @click="handleColumnHeadClick(col)">
              {{ col|capitalize }}
              <span
                class="sort-button"
                :class="getSortButtonClassname(col)"
              ></span>
            </div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="row in data" :key="row.id">
          <td v-for="(col, i) in schema" :key="i">
            {{ row[col] }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'MasterTable',
  components: {

  },
  props: {
    data: {
      type: Array,
      required: true,
    },
    settings: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {

    };
  },
  created() {

  },
  methods: {
    /**
     * Determine the adequate classname to a sort-button
     * @param {string} col the label of the column (e.g. a property from the data)
     * @returns {Array|null} An array of classes
     */
    getSortButtonClassname(col) {
      const { sortBy } = this.settings;

      if (!sortBy) {
        return null;
      }

      return [
        { 'sort-button--active': sortBy.key === col },
        { 'sort-button--desc': sortBy.key === col && sortBy.direction === -1 },
      ];
    },
    /**
     * On a column head click, update sortBy settings to sort by clicked label
     */
    handleColumnHeadClick(key) {
      const { sortBy } = this.settings;

      const newSortSettings = {
        key,
        direction: sortBy.key === key && sortBy.direction === 1 ? -1 : 1,
      };

      const settings = {
        ...this.settings,
        sortBy: newSortSettings,
      };

      this.$emit('updateSettings', settings);
    },
  },
  computed: {
    /**
     * Extrapole the table schema from the data properties,
     * without excludeKeys if needed
     */
    schema() {
      const schema = Object.keys(this.data[0]);

      if (this.settings.excludeKeys && this.settings.excludeKeys.length) {
        return schema.filter(key => !this.settings.excludeKeys.includes(key));
      }

      return schema;
    },
  },
  filters: {
    /**
     * Returns a word with its first letter capitalized
     * @param {string} value
     * @returns {string}
     */
    capitalize(value) {
      if (!value) {
        return '';
      }

      // eslint-disable-next-line no-param-reassign
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
  },
};
</script>

<style scoped lang="scss" src="./MasterTable.scss"></style>
