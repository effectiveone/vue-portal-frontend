<template>
  <form @submit.prevent="submitForm" class="form-container">
    <div class="input-group">
      <div class="form-control">
        <label for="firstName">First Name</label>
        <input type="text" id="firstName" v-model.trim="firstName" />
      </div>
      <div class="form-control">
        <label for="lastName">Last Name</label>
        <input type="text" id="lastName" v-model.trim="lastName" />
      </div>
    </div>
    <div class="form-control">
      <label for="email">Your E-Mail</label>
      <input type="email" id="email" v-model.trim="email" />
    </div>
    <div class="form-control">
      <label for="cv">CV (PDF file)</label>
      <input type="file" id="cv" accept="application/pdf" @change="handleFileUpload"/>
      <p v-if="!cv">Please select a file to upload.</p>
      <p v-if="fileSizeError">The file is too large. Maximum size is {{maxFileSize / 1000000}} MB.</p>
      <p v-if="fileTypeError">Please select a PDF file.</p>
    </div>
    <div class="form-control">
      <label for="message">Message</label>
      <textarea rows="5" id="message" v-model.trim="message"></textarea>
    </div>
    <p class="errors" v-if="!formIsValid">Please enter a valid email, non-empty message, and upload a PDF file under {{maxFileSize / 1000000}} MB.</p>
    <div class="actions">
      <base-button>Submit</base-button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      firstName: '',
      lastName: '',
      cv: null,
      fileSizeError: false,
      fileTypeError: false,
      message: '',
      formIsValid: true,
      maxFileSize: 5000000 // 5 MB
    };
  },
  methods: {
    submitForm() {
      this.formIsValid = true;
      if (
        this.email === '' ||
        !this.email.includes('@') ||
        this.message === '' ||
        !this.cv
      ) {
        this.formIsValid = false;
        return;
      }

      const formData = new FormData();
      formData.append('email', this.email);
      formData.append('firstName', this.firstName);
      formData.append('lastName', this.lastName);
      formData.append('cv', this.cv);

      this.$store.dispatch('requests/applyJob', {
  ...formData,
  id: this.$route.params.id 
});
      this.$router.replace('/jobs');

    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (!file) {
        this.cv = null;
        return;
      }
      if (file.size > this.maxFileSize) {
        this.fileSizeError = true;
        this.cv = null;
        return;
      }
      if (file.type !== 'application/pdf') {
        this.fileTypeError = true;
        this.cv = null;
        return;
      }
      this.cv = file;
      this.fileSizeError = false;
      this.fileTypeError = false;
    },
  },
};
</script>

<style scoped>
form {
  margin: 1rem;
  border: 1px solid #ccc;
  border-radius: 12px;
  padding: 1rem;
}

.form-control {
  margin: 0.5rem 0;
}

.input-group {
  display: grid;
  grid-template-columns: 1fr 1fr;

}

abel {
font-weight: bold;
margin-bottom: 0.5rem;
display: block;
width: 100%;
}

input,
textarea {
display: block;
width: 100%;
font: inherit;
border: 1px solid #ccc;
padding: 0.15rem;
margin-bottom: 0.5rem;
}

input:focus,
textarea:focus {
border-color: #3d008d;
background-color: #faf6ff;
outline: none;
}

.errors {
font-weight: bold;
color: red;
}

.actions {
text-align: center;
}
</style>
 
