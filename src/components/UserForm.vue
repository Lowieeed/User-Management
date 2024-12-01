<template>
    <div class="user-form card p-4">
      <form @submit.prevent="handleSubmit">
        <div class="mb-3">
          <label for="name" class="form-label">Name</label>
          <input
            id="name"
            type="text"
            v-model="user.name"
            class="form-control"
            placeholder="Enter Name"
            required
          />
        </div>
        <div class="mb-3">
          <label for="email" class="form-label">Email</label>
          <input
            id="email"
            type="email"
            v-model="user.email"
            class="form-control"
            placeholder="Enter Email"
            required
          />
        </div>
        <button type="submit" class="btn btn-primary w-100">
          {{ user.id ? 'Update' : 'Add' }} User
        </button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: ['userToEdit'],
    data() {
      return {
        user: this.userToEdit
          ? { ...this.userToEdit }
          : { name: '', email: '' },
      };
    },
    watch: {
      userToEdit: {
        immediate: true,
        handler(newValue) {
          this.user = newValue ? { ...newValue } : { name: '', email: '' };
        },
      },
    },
    methods: {
      handleSubmit() {
        if (this.user.id) {
          this.$emit('update-user', this.user);
        } else {
          this.$emit('add-user', { ...this.user });
        }
        this.user = { name: '', email: '' };
      },
    },
  };
  </script>
  