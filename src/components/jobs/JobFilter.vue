<template>
  <base-card>
    <h2>Find Your Dream Job</h2>
    <div class="filter-section">
      <h3>Filter by Job Title:</h3>
      <div class="filter-option">
        <input type="checkbox" id="frontend" v-model="selectedTitles" value="Frontend Developer" />
        <label for="frontend">Frontend Developer</label>
      </div>
      <div class="filter-option">
        <input type="checkbox" id="backend" v-model="selectedTitles" value="Backend Developer" />
        <label for="backend">Backend Developer</label>
      </div>
      <div class="filter-option">
        <input type="checkbox" id="mobile" v-model="selectedTitles" value="Mobile Developer" />
        <label for="mobile">Mobile Developer</label>
      </div>
    </div>
    <div class="filter-section">
      <h3>Sort by:</h3>
      <div class="filter-option">
        <input type="radio" id="location" v-model="sortBy" value="location" />
        <label for="location">Location</label>
      </div>
      <div class="filter-option">
        <input type="radio" id="salary" v-model="sortBy" value="salary" />
        <label for="salary">Salary</label>
      </div>
    </div>
  </base-card>
</template>

<script>
export default {
  emits: ['change-filter'],
  data() {
    return {
      selectedTitles: ['Frontend Developer', 'Backend Developer', 'Mobile Developer'],
      sortBy: 'location',
    };
  },
  watch: {
    selectedTitles() {
      this.emitFilters();
    },
    sortBy() {
      this.emitFilters();
    },
  },
  methods: {
    emitFilters() {
      const filters = {
        jobTitles: this.selectedTitles,
        sortBy: this.sortBy,
      };
      this.$emit('change-filter', filters);
    },
  },
};
</script>

<style scoped>
h2 {
  margin: 0.5rem 0;
}

.filter-section {
  margin-bottom: 1rem;
}

.filter-section h3 {
  margin-bottom: 0.5rem;
}

.filter-option {
  display: inline-flex;
  align-items: center;
  margin-right: 1rem;
}

.filter-option label,
.filter-option input {
  vertical-align: middle;
}

.filter-option label {
  margin-left: 0.25rem;
}
</style>
