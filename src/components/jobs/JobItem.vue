<template>
  <li>
    <h3>{{ jobTitle }}</h3>
    <h4>${{ rate }}/hour</h4>
    <div>
      <base-badge
        v-for="technology in technologies"
        :key="technology.name"
        :type="technology.type"
        :title="technology.name"
      ></base-badge>
    </div>
    <div>
      {{ location }}
    </div>
    <div class="actions">
      <base-button mode="outline" link :to="jobContactLink">Contact</base-button>
      <base-button link :to="jobDetailsLink">View Details</base-button>
    </div>
  </li>
</template>

<script>
export default {
  props: ['id', 'jobTitle', 'rate', 'technologies', 'location', 'description'],
  computed: {
    jobContactLink() {
      return this.$route.path + '/' + this.id + '/contact';
    },
    jobDetailsLink() {
      return this.$route.path + '/' + this.id;
    },
  },
};
</script>

<style scoped>
li {
  margin: 1rem 0;
  border: 1px solid #424242;
  border-radius: 12px;
  padding: 1rem;
}

h3 {
  font-size: 1.5rem;
}

h3,
h4 {
  margin: 0.5rem 0;
}

div {
  margin: 0.5rem 0;
}

.actions {
  display: flex;
  justify-content: flex-end;
}

.frontend {
  background-color: #3d008d;
  color: white;
}

.backend {
  background-color: #71008d;
  color: white;
}

.mobile {
  background-color: #8d006e;
  color: white;
}
</style>
