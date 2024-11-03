

<template>
  <div id="app">
    <h1>User Management</h1>
    <UserManagement />
  </div>
</template>

<script>
import UserManagement from './components/UserManagement.vue';

export default {
  name: 'App',
  components: {
    UserManagement
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}

  h1{
    color: white;
  }
</style>
