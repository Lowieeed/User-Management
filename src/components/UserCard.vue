<template>
    <div class="user-card card shadow-sm p-3">
      <h5 class="card-title">{{ user.name }}</h5>
      <p class="card-text">{{ user.email }}</p>
      <div class="d-flex justify-content-between">
        <button class="btn btn-warning btn-sm" @click="$emit('edit-user', user)">
          Edit
        </button>
        <button class="btn btn-danger btn-sm" @click="$emit('delete-user', user.id)">
          Delete
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['user'],
  };
  </script>
  
  <style scoped>
  .user-card {
    border-radius: 10px;
  }
  </style>
  