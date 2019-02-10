<template>
  <div id="app">
    <SideColumn />
    <MainColumnContainer />
  </div>
</template>

<script>
import SideColumn from './ui/components/SideColumn/SideColumn.vue';
import MainColumnContainer from './ui/containers/MainColumnContainer.vue';

export default {
  name: 'app',
  components: {
    SideColumn,
    MainColumnContainer,
  },
};
</script>

<style lang="scss" src="./assets/sass/main.scss"></style>
