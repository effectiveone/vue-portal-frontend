<template>
  <div>
    <section>
      <base-card>
        <h2>{{ jobTitle }}</h2>
        <h3>${{ rate }}/hour</h3>
      </base-card>
    </section>
    <section>
      <base-card>
        <header>
          <h2>Interested? Apply now!</h2>
          <base-button link :to="applyLink">Apply</base-button>
        </header>
        <router-view :id="id"></router-view> 
       </base-card>
    </section>
    <section>
      <base-card>
        <base-badge v-for="tech in technologies" :key="tech.name" :type="tech.level" :title="tech.name"></base-badge>
        <p>{{ description }}</p>
      </base-card>
    </section>
    <section>
      <base-card>
        <header>
          <h2>Location(s)</h2>
        </header>
        <ul>
          <li v-for="location in locations" :key="location">{{ location }}</li>
        </ul>
      </base-card>
    </section>
  </div>
</template>

<script>
export default {
  props: ['id'],
  computed: {
    jobTitle() {
      return this.selectedJob.jobTitle;
    },
    technologies() {
      return this.selectedJob.technologies;
    },
    description() {
      return this.selectedJob.description;
    },
    rate() {
      return this.selectedJob.rate;
    },
    locations() {
      return this.selectedJob.locations;
    },
    applyLink() {
      return this.$route.path + '/' + this.id + '/apply';
    },
  },
  data() {
    return {
      selectedJob: null,
    };
  },
  created() {
    this.selectedJob = this.$store.getters['jobs/jobs'].find(
      (job) => job.id === this.id
    );
  },
};
</script>
