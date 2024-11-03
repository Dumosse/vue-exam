<template>
  <div class="user-management">
    <form @submit.prevent="handleNewUserSubmit">
      <div class="nameInput">
        <label for="name">Name</label>
        <input v-model="newUser.name" placeholder="Enter name..." required />
      </div>
      <div class="emailInput">
        <label for="email">Email</label>
        <input v-model="newUser.email" placeholder="Enter email..." required type="email" />
      </div>
      <button type="submit" class="submit">Add User</button>
    </form>

    <div v-if="users.length" class="user-list">
      <div v-for="user in users" :key="user.id" class="user-card">
        <div class="userDetails">
        <p><strong>Name:</strong> {{ user.name }}</p>
        <p><strong>Email:</strong> {{ user.email }}</p>
        </div>
        <div class="btnCard">
          <button @click="openEditModal(user)" class="edit">Edit</button>
          <button @click="deleteUser(user.id)" class="delete">Delete</button>
        </div>
      </div>
    </div>
    <p v-else class="noUser">No users available</p>

    <div v-if="showModal" class="modal-overlay">
      <div class="modal-content">
        <h3>Edit User</h3>
        <form @submit.prevent="handleUserSubmit">
          <div class="nameInput">
          <label for="name">Name</label>
          <input v-model="currentUser.name" placeholder="Enter name..." required />
          </div>
          <div class="emailInput">
          <label for="email">Email</label>
          <input v-model="currentUser.email" placeholder="Enter email..." required type="email" />
          </div>
          <button type="submit" class="submit">Update User</button>
          <button type="button" @click="closeModal" class="cancelBtn">Cancel</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      users: [],
      newUser: { name: "", email: "" },
      currentUser: { id: null, name: "", email: "" },
      isEditing: false,
      showModal: false
    };
  },
  methods: {
    handleNewUserSubmit() {
      const nameExists = this.users.some(
        user => user.name.toLowerCase() === this.newUser.name.toLowerCase()
      );
      const emailExists = this.users.some(
        user => user.email.toLowerCase() === this.newUser.email.toLowerCase()
      );

      if (nameExists) {
        alert("This name already exists.");
        return;
      }
      if (emailExists) {
        alert("This email already exists.");
        return;
      }

      this.users.push({
        ...this.newUser,
        id: Date.now()
      });

      this.resetNewUserForm();
    },

    handleUserSubmit() {
      const nameExists = this.users.some(
        user =>
          user.name.toLowerCase() === this.currentUser.name.toLowerCase() &&
          user.id !== this.currentUser.id
      );
      const emailExists = this.users.some(
        user =>
          user.email.toLowerCase() === this.currentUser.email.toLowerCase() &&
          user.id !== this.currentUser.id
      );

      if (nameExists) {
        alert("This name already exists.");
        return;
      }
      if (emailExists) {
        alert("This email already exists.");
        return;
      }

      const index = this.users.findIndex(user => user.id === this.currentUser.id);
      if (index !== -1) {
        this.users.splice(index, 1, { ...this.currentUser });
      }

      this.closeModal();
    },

    openEditModal(user) {
      this.currentUser = { ...user };
      this.isEditing = true;
      this.showModal = true;
    },

    closeModal() {
      this.showModal = false;
      this.resetEditForm();
    },

    deleteUser(id) {
      this.users = this.users.filter(user => user.id !== id);
    },

    resetNewUserForm() {
      this.newUser = { name: "", email: "" };
    },

    resetEditForm() {
      this.currentUser = { id: null, name: "", email: "" };
      this.isEditing = false;
    }
  }
};
</script>

<style scoped>
.user-management {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.user-management input {
  display: flex;
  flex-direction: column;
}

button{
  outline: none;
  box-shadow: none;
  border: none;
}

.noUser{
  color: white;
}

.nameInput{
  display: flex;
  flex-direction: column;
  gap: 2px;
  margin-bottom: 10px;
}

.nameInput label{
  align-self: flex-start;
  color: rgb(191, 187, 187);
}

.nameInput input{
  padding: 6px;
}

.emailInput{
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.emailInput label{
  align-self: flex-start;
  color: rgb(191, 187, 187);
}

.emailInput input{
  padding: 6px;
}

.submit {
  margin: 5px;
  padding: 14px;
  border-radius: 6px;
  background-color: #006FEE;
  width: 100px;
  align-self: flex-end;
  color: white;
  text-decoration: none;
  margin-top: 10px;
}

.cancelBtn {
  margin: 5px;
  padding: 5px;
  border-radius: 6px;
  background-color: rgb(227, 54, 54);
  color: white;
  text-decoration: none;
}

.btnCard {
  display: flex;
  flex-direction: row;
  gap: 10px;
  margin-top: 10px;
}

.userDetails{
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 10px;
}

.userDetails p{
  margin: 0;
}

.edit {
  margin: 5px;
  padding: 14px;
  border-radius: 6px;
  background-color: #006FEE;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 82px;
  color: white;
  text-decoration: none;
}

.delete {
  margin: 5px;
  padding: 14px;
  border-radius: 6px;
  background-color: #FF5656;
  width: 82px;
  color: white;
  text-decoration: none;
}

.user-list {
  margin-top: 20px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 10px;
  align-items: flex-start;
  justify-content: flex-start;
  width: 100%;
}

.user-card {
  display: flex;
  flex: 1 0 200px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #f9f9f9;
  border: 1px solid #ddd;
  padding: 10px;
  max-width: 210px;
  max-height: 200px;
  width: 100%;
  height: 100%;
  border-radius: 5px;
  margin-bottom: 10px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 6px;
  width: 300px;
  max-width: 100%;
}

.modal-content .nameInput{
  display: flex;
  flex-direction: column;
  gap: 2px;
  margin-bottom: 10px;
}

.modal-content .nameInput label{
  align-self: flex-start;
  color: rgb(65, 63, 63);
}

.modal-content .nameInput input{
  padding: 6px;
}

.modal-content .emailInput{
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.modal-content .emailInput label{
  align-self: flex-start;
  color: rgb(65, 63, 63);
}

.modal-content .emailInput input{
  padding: 6px;
}

.modal-content .cancelBtn{
  margin: 5px;
  padding: 14px;
  border-radius: 6px;
  background-color: #FF5656;
  width: 82px;
  color: white;
  text-decoration: none;
}

.modal-content .submit{
  margin: 5px;
  padding: 14px;
  border-radius: 6px;
  background-color: #006FEE;
  width: 120px;
  align-self: flex-end;
  color: white;
  text-decoration: none;
  margin-top: 10px;
}
</style>
