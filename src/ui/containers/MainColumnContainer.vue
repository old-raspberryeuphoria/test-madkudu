<template>
  <MainColumn
    :antelopesData="antelopesData"
    :isLoading="isLoading"
  />
</template>

<script>
import MainColumn from '@/ui/components/MainColumn/MainColumn.vue';

export default {
  name: 'MainColumnContainer',
  components: {
    MainColumn,
  },
  data() {
    return {
      antelopesData: null,
      isLoading: true,
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    /**
     * Fetch data from API
     * The proxy is here to allow fetching from localhost
     */
    fetchData() {
      const proxyurl = 'https://cors-anywhere.herokuapp.com/';
      const dataURI = 'https://s3-us-west-2.amazonaws.com/team.madkudu.com/species.json';

      fetch(proxyurl + dataURI)
        .then(res => res.json())
        .then((data) => {
          // Add an id to each antelope
          this.antelopesData = data.map((antelope, id) => ({
            ...antelope,
            id,
          }));

          this.isLoading = false;
        });
    },
  },
};
</script>
