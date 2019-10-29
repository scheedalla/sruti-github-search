<template>
  <div id="app" class="container">
    <h1>Search Github Users by Name</h1>

    <search-form @search:records="search" />
    <search-results :records="records" :search-val="searchVal" :page="page" :per-page="perpage" />
  </div>
</template>

<script>
import SearchResults from "@/components/SearchResults.vue";
import SearchForm from "@/components/SearchForm.vue";

export default {
  name: "app",
  components: {
    SearchResults,
    SearchForm
  },
  data() {
    return {
      records: {},
      items: [],
      searchVal: "",
      page: 1,
      perpage: 10
    };
  },
  methods: {
    /* GET request to Github users earch */
    async search(searchVal) {
      try {
        const response = await fetch(
          "https://api.github.com/search/users?q=" +
            searchVal +
            "&page=" +
            this.page +
            "&per_page=" +
            this.perpage
        );
        const data = await response.json();
        this.records = data;
        this.items = data.items;
        this.searchVal = searchVal;
      } catch (error) {
        // console.error(error);
      }
    }
  }
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
