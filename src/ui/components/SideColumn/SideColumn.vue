<template>
  <aside
    class="side-column"
    :class="{ 'side-column--right': side === 'right' }"
  >
    <nav>
      <ul class="main-navigation">
        <li><a href="#overview">Overview</a></li>
        <li>
          <a class="inactive">Graphics</a>
          <ul class="sub-navigation">
            <li><a href="#hornsByContinents">Horns by continents</a></li>
          </ul>
        </li>
      </ul>
    </nav>
  </aside>
</template>

<script>
export default {
  name: 'SideColumn',
  props: {
    side: {
      type: String,
      default: 'left',
    },
  },
};
</script>

<style scoped lang="scss" src="./SideColumn.scss"></style>
