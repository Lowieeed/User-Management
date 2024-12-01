<template>
  <div id="app" class="container mt-5">
    <h1 class="text-center mb-4">User Management Application</h1>

    <!-- User Form -->
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div
          :class="['card', { 'bg-light': userToEdit }]">
          <div class="card-body">
            <h5 class="card-title text-center">
              {{ userToEdit ? "Update User" : "Add User" }}
            </h5>
            <form @submit.prevent="handleSubmit">
              <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input
                  type="text"
                  id="name"
                  class="form-control"
                  v-model="formData.name"
                  required
                />
              </div>

              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input
                  type="email"
                  id="email"
                  class="form-control"
                  v-model="formData.email"
                  @input="checkEmail"
                  required
                />
                <!-- Email Exists Error -->
                <div v-if="emailExists" class="text-danger mt-1">
                  Email already exists!
                </div>
              </div>

              <div class="d-flex justify-content-between">
                <button
                  type="submit"
                  class="btn btn-primary"
                  :disabled="emailExists || !formData.name || !formData.email"
                >
                  {{ userToEdit ? "Update" : "Add" }}
                </button>
                <button
                  v-if="userToEdit"
                  type="button"
                  class="btn btn-secondary"
                  @click="cancelEdit"
                >
                  Cancel
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- User List -->
    <div class="row mt-4">
      <div
        v-for="user in users"
        :key="user.id"
        class="col-md-4"
      >
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ user.name }}</h5>
            <p class="card-text">{{ user.email }}</p>
            <button class="btn btn-warning me-2" @click="editUser(user)">
              Edit
            </button>
            <button class="btn btn-danger" @click="deleteUser(user.id)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [], // List Rendering
      formData: { name: "", email: "" }, // Reactive Variables
      userToEdit: null, // Conditional Rendering
      emailExists: false, // Conditional Rendering
    };
  },
  methods: {
    handleSubmit() {
      if (this.userToEdit) {
        // Update existing user
        const index = this.users.findIndex((user) => user.id === this.userToEdit.id);
        if (index !== -1) {
          this.users[index] = { ...this.formData, id: this.userToEdit.id };
        }
      } else {
        // Add new user
        const newUser = { ...this.formData, id: Date.now() };
        this.users.push(newUser);
      }
      this.resetForm();
    },
    editUser(user) {
      this.userToEdit = { ...user }; // Edit functionality
      this.formData = { ...user }; // Form Binding
    },
    deleteUser(userId) {
      this.users = this.users.filter((user) => user.id !== userId); // Delete functionality
    },
    checkEmail() {
      this.emailExists = this.users.some(
        (user) =>
          user.email === this.formData.email &&
          (!this.userToEdit || user.id !== this.userToEdit.id)
      );
    },
    cancelEdit() {
      this.resetForm();
    },
    resetForm() {
      this.userToEdit = null; // Conditional Rendering
      this.formData = { name: "", email: "" }; // Reset data binding
      this.emailExists = false;
    },
  },
};
</script>

<style>
.card {
  border-radius: 8px;
}
</style>
