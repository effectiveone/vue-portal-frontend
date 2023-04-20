<template>
  <div >
    <JobFilter @change-filter="setFilters" />
    <div >
    <base-card>
      <template v-if="!isLoading">
        <template v-if="hasJobs" >
          <job-item v-for="job in filteredJobs" :key="job.id" v-bind="job"></job-item>
        </template>
        <template v-else>
          <p>No jobs available.</p>
        </template>
      </template>
      <template v-else>
        <p>Loading...</p>
      </template>
    </base-card>
    </div>
  </div>
</template>

<script>
import JobItem from '@/components/jobs/JobItem';
import JobFilter from '@/components/jobs/JobFilter';

export default {
  components: {
    JobItem,
    JobFilter,
  },
  data() {
    return {
      isLoading: false,
      error: null,
      activeFilters: {
        frontend: true,
        backend: true,
        mobile: true,
      },
    };
  },
  computed: {
    isLoggedIn() {
      return this.$store.getters['auth/isAuthenticated'];
    },
    isApplicant() {
      return this.$store.getters['jobs/isApplicant'];
    },
    filteredJobs() {
      const jobs = this.$store.getters['jobs/jobs'];
      return jobs.filter((job) => {
        return this.activeFilters.frontend && job.jobTitle.toLowerCase().includes('frontend developer') ||
          this.activeFilters.backend && job.jobTitle.toLowerCase().includes('backend developer') ||
          this.activeFilters.mobile && job.jobTitle.toLowerCase().includes('mobile developer');
      });
    },
    hasJobs() {
      return !this.isLoading && this.$store.getters['jobs/hasJobs'];
    },
  },
  created() {
    console.log(this.$store.state); // Wyświetla cały stan store
    console.log(this.$store.state.jobs); // Wyświetla stan tylko modułu jobs
    this.loadJobs();
  },

  methods: {
    setFilters(updatedFilters) {
      this.activeFilters.frontend = updatedFilters.jobTitles.includes('Frontend Developer');
      this.activeFilters.backend = updatedFilters.jobTitles.includes('Backend Developer');
      this.activeFilters.mobile = updatedFilters.jobTitles.includes('Mobile Developer');
    },
    async loadJobs(refresh = false) {
      this.isLoading = true;
      try {
        await this.$store.dispatch('jobs/loadJobs', {
          forceRefresh: refresh,
        });
      } catch (error) {
        this.error = error.message || 'Something went wrong!';
      }
      this.isLoading = false;
    },
    handleError() {
      this.error = null;
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.controls {
  display: flex;
  justify-content: space-between;
}
.container {
  position: relative
}
.job-list {
width: 500px;
display: flex;
flex-direction: column;
align-items: flex-end;
}
</style>