<template>
  <section>
    <base-card>
      <h2>Add new job!</h2>
      <JobsForm @save-data="saveData"></JobsForm>
    </base-card>
  </section>
</template>


<script>
import JobsForm from '../../components/jobs/JobsForm.vue';

export default {
  components: {
    JobsForm,
  },
  methods: {
    saveData(data) {
      this.$store.dispatch('jobs/registerJob', data);
      this.$router.replace('/jobs');
    }
  }
};
</script>