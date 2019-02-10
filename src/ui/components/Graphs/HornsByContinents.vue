<template>
  <div class="graph-horns-per-continent">
    <h2>Antelopes horns type by continent</h2>
    <p class="graph-paragraph">
      This graphic shows the repartition of horn-types per continent. As you can see, Africa has <strong>{{ continents.Africa.hornsTypesLength }}</strong> different horn-types, while Asia has only <strong>{{ continents.Asia.hornsTypesLength }}</strong>.
    </p>
    <div class="graph-wrapper">
      <div class="graph-results">
        <div
          class="graph-continent-wrapper"
          v-for="(continent, i) in Object.keys(continents)"
          :key="i"
        >
          <div class="graph-continent-content">
            <div
              v-for="hornsType in continents[continent].data"
              class="graph-horns-type"
              :class="{
                'graph-horns-type--inactive': currentlyHovered && currentlyHovered !== hornsType.name
              }"
              :style="{
                height: hornsType.percentage,
                backgroundColor: hornsColor[hornsType.name],
              }"
              :key="hornsType.id"
              :title="hornsType.name"
            >
              <span class="graph-horns-type-label">
                {{ hornsType[currentModel] }}
              </span>
            </div>
          </div>
          <span class="graph-continent-label">{{ continent }}</span>
        </div>
      </div>
      <div class="graph-legend">
        <div class="graph-buttons">
          <span
            v-for="(model, i) in modelButtons"
            class="graph-button"
            :class="{ 'graph-button--active': currentModel === model}"
            :key="i"
            @click="currentModel = model"
          >
            {{ model|capitalize }}
          </span>
        </div>
        <div
          v-for="(hornsType, i) in hornsTypes"
          :key="i"
          @mouseenter="handleLegendHover(hornsType)"
          @mouseleave="handleLegendHover(hornsType, true)"
          class="legend-content"
        >
          <div
            class="legend-square"
            :style="{
              backgroundColor: hornsColor[hornsType],
            }"
          ></div>
          {{ hornsType }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HornsByContinents',
  props: {
    data: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      continents: [],
      hornsTypes: [],
      currentlyHovered: null,
      currentModel: 'percentage',
      modelButtons: ['percentage', 'count'],
    };
  },
  created() {
    this.initGraphData();
  },
  methods: {
    /**
     * This is a helper method to calculate a percentage
     * If this was not a POC, it should be moved to ./src/ui/helpers
     */
    calculatePercentage(value, total) {
      return Math.round((100 * value) / total * 10) / 10;
    },
    /**
     * Group antelopes by continents,
     * then count the horns type in each continent
     */
    initGraphData() {
      const continents = {};
      const hornsTypes = [];

      this.data.forEach((row) => {
        const { continent, horns } = row;

        if (!Object.prototype.hasOwnProperty.call(continents, continent)) {
          // Set a new continent if it is not set yet,
          // then set the current horns type for this continent
          continents[continent] = {
            hornsTypesLength: 1,
            antelopesLength: 1,
          };

          continents[continent][horns] = 1;
        } else {
          if (!Object.prototype.hasOwnProperty.call(continents[continent], horns)) {
            // Set a new type of horns if there this continent does not have it already
            continents[continent][horns] = 1;
            continents[continent].hornsTypesLength += 1;
          } else {
            // Otherwise, increment it
            continents[continent][horns] += 1;
          }

          continents[continent].antelopesLength += 1;
        }

        if (hornsTypes.indexOf(horns) === -1) {
          hornsTypes.push(horns);
        }
      });

      // Create an ordered array from the raw data
      Object.values(continents).forEach((continent) => {
        // eslint-disable-next-line no-param-reassign
        continent.data = [];

        hornsTypes.forEach((type, i) => {
          if (continent[type]) {
            const { calculatePercentage } = this;

            const value = {
              id: i,
              name: type,
              count: continent[type],
              get percentage() {
                return `${calculatePercentage(this.count, continent.antelopesLength)}%`;
              },
            };

            continent.data.push(value);
          }
        });

        continent.data.sort((a, b) => a.count - b.count);
      });

      this.continents = continents;
      this.hornsTypes = hornsTypes;
    },
    /**
     * When hovering legend, highlight the corresponding field on the graph
     * by setting (or unsetting) the currentlyHovered property
     * @param {string} hornsType the hovered horns-type
     * @param {boolean} exit if true, unset currentlyHovered
     */
    handleLegendHover(hornsType, exit = false) {
      this.currentlyHovered = exit ? null : hornsType;
    },
  },
  computed: {
    /**
     * Defines a color scheme
     */
    colorScheme() {
      return [
        '#4757a9',
        '#f5935a',
        '#824bda',
        '#fe6a6a',
        '#7b5050',
        '#65ca8d',
      ];
    },
    /**
     * Attributes a color to each horns type
     */
    hornsColor() {
      const hornsColor = {};

      this.hornsTypes.forEach((type, i) => {
        hornsColor[type] = this.colorScheme[i];
      });

      return hornsColor;
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

<style scoped lang="scss" src="./Graphs.scss"></style>
