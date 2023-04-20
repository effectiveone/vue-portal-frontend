<template>
  <form @submit.prevent="submitForm">
    <div class="form-control" :class="{invalid: !jobTitle.isValid}">
      <label for="job-title">Job Title</label>
      <select id="job-title" v-model.trim="jobTitle.val" @blur="clearValidity('jobTitle')">
        <option disabled value="">Select Job Title</option>
        <option value="Mobile Developer">Mobile Developer</option>
        <option value="Frontend Developer">Frontend Developer</option>
        <option value="Backend Developer">Backend Developer</option>
      </select>
      <p v-if="!jobTitle.isValid">Job Title must be selected.</p>
    </div>
    <div class="form-control" :class="{invalid: !jobDescription.isValid}">
      <label for="job-description">Job Description</label>
      <textarea
        id="job-description"
        rows="5"
        v-model.trim="jobDescription.val"
        @blur="clearValidity('jobDescription')"
      ></textarea>
      <p v-if="!jobDescription.isValid">Job Description must not be empty.</p>
    </div>
    <div class="form-control" :class="{invalid: !companyName.isValid}">
      <label for="company-name">Company Name</label>
      <input
        type="text"
        id="company-name"
        v-model.trim="companyName.val"
        @blur="clearValidity('companyName')"
      />
      <p v-if="!companyName.isValid">Company Name must not be empty.</p>
    </div>
    <div class="form-control" :class="{invalid: !location.isValid}">
      <label for="location">Location</label>
      <input
        type="text"
        id="location"
        v-model.trim="location.val"
        @blur="clearValidity('location')"
      />
      <p v-if="!location.isValid">Location must not be empty.</p>
    </div>
    <div class="form-control" :class="{invalid: !technologies.isValid}">
      <h3>Technologies</h3>
      <div v-for="(technology, index) in technologies.val" :key="index">
        <input
          type="text"
          v-model.trim="technology.name"
          @blur="clearValidity(`technology${index}`)"
        />
        <select v-model="technology.level" @blur="clearValidity(`technology${index}`)">
          <option disabled value="">Select Level</option>
          <option value="Beginner">Beginner</option>
          <option value="Intermediate">Intermediate</option>
          <option value="Advanced">Advanced</option>
        </select>
        <p v-if="!technology.name || !technology.level">
          Technology Name and Level must not be empty.
        </p>
      </div>
      <div class="form-control" :class="{invalid: !technologies.isValid}">
        <button type="button" @click="addTechnology">Add Technology</button>
        <p v-if="!technologies.isValid">At least one technology must be added.</p>
      </div>
    </div>
    <div class="form-control" :class="{invalid: !salary.isValid}">
      <label for="salary">Salary</label>
      <input type="number" id="salary" v-model.number="salary.val" @blur="clearValidity('salary')" />
      <p v-if="!salary.isValid">Salary must be greater than or equal to 0.</p>
    </div>
    <p v-if="!formIsValid">Please fixthe above errors and submit again.</p>
<base-button @click="submitForm">Submit</base-button>

  </form>
</template>
<script>
export default {
  emits: ['save-data'],
  data() {
    return {
      jobTitle: {
        val: '',
        isValid: true,
      },
      jobDescription: {
        val: '',
        isValid: true,
      },
      companyName: {
        val: '',
        isValid: true,
      },
      location: {
        val: '',
        isValid: true,
      },
      technologies: {
        val: [{ name: '', level: '', isValid: true }],
        isValid: true,
      },
      salary: {
        val: null,
        isValid: true,
      },
      formIsValid: true,
    };
  },
  methods: {
    clearValidity(input) {
  if (input in this) {
    this[input].isValid = true;
  }
},
    validateForm() {
      this.formIsValid = true;
      if (this.jobTitle.val === '') {
        this.jobTitle.isValid = false;
        this.formIsValid = false;
      }
      if (this.jobDescription.val === '') {
        this.jobDescription.isValid = false;
        this.formIsValid = false;
      }
      if (this.companyName.val === '') {
        this.companyName.isValid = false;
        this.formIsValid = false;
      }
      if (this.location.val === '') {
        this.location.isValid = false;
        this.formIsValid = false;
      }
      if (
        this.technologies.val.some(
          (technology) => !technology.name || !technology.level
        )
      ) {
        this.technologies.isValid = false;
        this.formIsValid = false;
      }
      if (this.salary.val === null || this.salary.val < 0) {
        this.salary.isValid = false;
        this.formIsValid = false;
      }
    },
    submitForm() {
      this.validateForm();
      if (!this.formIsValid) {
        return;
      }

      const formData = {
        jobTitle: this.jobTitle.val,
        description: this.jobDescription.val,
        companyName: this.companyName.val,
        location: this.location.val,
        technologies: this.technologies.val,
        rate: this.salary.val,
      };

      this.$emit('save-data', formData);

      // Reset form after submission
      this.jobTitle.val = '';
      this.jobDescription.val = '';
      this.companyName.val = '';
      this.location.val = '';
      this.technologies.val = [{ name: '', level: '', isValid: true }];
      this.salary.val = null;
      this.formIsValid = true;
    },
    addTechnology() {
      this.technologies.val.push({ name: '', level: '', isValid: true });
    },

    removeTechnology(index) {
      this.technologies.val.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input[type='checkbox'] + label {
  font-weight: normal;
  display: inline;
  margin: 0 0 0 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  border: 1px solid #ccc;
  font: inherit;
}

input:focus,
textarea:focus {
  background-color: #f0e6fd;
  outline: none;
  border-color: #3d008d;
}

input[type='checkbox'] {
  display: inline;
  width: auto;
  border: none;
}

input[type='checkbox']:focus {
  outline: #3d008d solid 1px;
}

h3 {
  margin: 0.5rem 0;
  font-size: 1rem;
}

.invalid label {
  color: red;
}

.invalid input,
.invalid textarea {
  border: 1px solid red;
}

button[type='button'] {
  margin-top: 0.5rem;
}
</style>