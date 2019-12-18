<template>
  <div id="employee-form" class="container">
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="employee-name">Employee Name</label>
        <input
          type="text"
          ref="first"
          name="name"
          id="employee-name"
          class="form-control"
          :class="{'has-error': submitting && invalidName}"
          v-model="employee.name"
          @focus="clearStatus"
          @keypress="clearStatus"
        />
      </div>
      <div class="form-group">
        <label for="employee-email">Employee Email</label>
        <input
          type="email"
          name="email"
          id="employee-email"
          class="form-control"
          v-model="employee.email"
          :class="{'has-error': submitting && invalidEmail}"
        />
      </div>

      <!-- Error place -->
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>

      <input type="submit" value="Add Employee" class="btn btn-primary" />
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      //   set submitting to true and clear success and error statuses
      this.submitting = true;
      this.clearStatus();

      //   check whether name or email are invalid and set error to true
      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }

      //   emit the add employee event
      this.$emit("add:employee", this.employee);

      // focus on the first field
      this.$refs.first.focus();

      // clear form
      this.employee = {
        name: "",
        email: ""
      };

      // set error and submitting to false, success to true
      this.error = false;
      this.submitting = false;
      this.success = true;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },
    invalidEmail() {
      return this.employee.email === "";
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 20px;
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