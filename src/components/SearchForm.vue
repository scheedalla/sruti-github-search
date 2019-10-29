<template>
  <div id="search-form">
    <form @submit.prevent="handleSubmit">
      <input
        ref="first"
        type="text"
        v-model="searchVal"
        @focus="clearStatus"
        @keypress="clearStatus"
        placeholder="github"
        required
      />

      <button>Search</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "search-form",
  data() {
    return {
      searchVal: ""
    };
  },
  methods: {
    handleSubmit() {
      this.$emit("search:records", this.searchVal);
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidDate() {
      return this.record.date === "";
    },

    invalidWaketime() {
      return this.record.waketime === "";
    },
    invalidNapStartTime() {
      return this.record.napStartTime === "";
    },
    invalidNapEndTime() {
      return this.record.napEndTime === "";
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>