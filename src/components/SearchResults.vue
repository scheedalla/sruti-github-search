<template>
  <div id="search-results" v-if="records.total_count>0">
    <div>
      <div>
        <button @click="prevPage">&#x2190; prev</button>&nbsp;
        <button @click="nextPage">next &#x2192;</button>
      </div>
      <div>
        <span v-if="records.total_count>0">
          <strong>{{records.total_count}} users</strong>
        </span>
        <span v-else>0</span>
        (Page {{pageNumber}}, Items per page: {{perPage}})
      </div>
      <div v-for="(item, index) in results.items" :key="item.id" class="item">
        <div>Item {{itemNumber(index)}}</div>
        <a v-bind:href="item.html_url" target="_blank">
          <div v-for="(v,i) in item" :key="v.id">
            <span v-if="i==='avatar_url'">
              <img v-bind:src="v" class="responsive-image" />
            </span>
            <span v-else>
              <strong>{{i}}:</strong>
              {{v}}
            </span>
          </div>
          <br />
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "search-results",
  props: {
    records: { type: Object },
    size: {
      type: Number,
      required: false,
      default: 10
    },
    searchVal: String,
    page: Number,
    perPage: Number
  },
  data() {
    return {
      editing: null,
      pageNumber: 1
    };
  },
  methods: {
    nextPage() {
      this.pageNumber++;
      this.getPaginatedData();
    },
    prevPage() {
      this.pageNumber--;
      this.getPaginatedData();
    },
    async getPaginatedData() {
      /* GET request to get the next or previous page data */
      try {
        const response = await fetch(
          "https://api.github.com/search/users?q=" +
            this.searchVal +
            "&page=" +
            this.pageNumber +
            "&per_page=" +
            this.perPage
        );
        const data = await response.json();
        this.results = data;
      } catch (error) {
        // console.error(error);
      }
    },
    itemNumber(index) {
      // this returns the number of the item in the list of users
      return (this.pageNumber - 1) * 10 + (index + 1);
    }
  },
  computed: {
    results() {
      return this.records;
    }
  }
};
</script>

<style scoped>
a {
  color: black;
}
a:hover {
  text-decoration: none;
}

.item {
  border: 1px solid black;
  padding: 10px;
  margin-bottom: 10px;
}
</style>