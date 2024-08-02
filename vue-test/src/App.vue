<template>
  <div id="app">
    <div class="header">
      <button class="add-button" @click="toggleModal">Добавить</button>
    </div>
    <UserTable :users="users" />
    <Modal :isVisible="isModalVisible" @close="toggleModal">
      <UserForm :users="users" @submit="handleSubmit" />
    </Modal>
  </div>
</template>

<script>
import UserTable from "./components/UserTable.vue";
import UserForm from "./components/UserForm.vue";
import Modal from "./components/Modal.vue";

export default {
  components: {
    UserTable,
    UserForm,
    Modal,
  },
  data() {
    return {
      users: [
        { id: 1, name: "MIKE TYSON", phone: "+88005553535" },
        { id: 2, name: "Эльвира Набиуллина", phone: "+181818181818" },
      ],
      isModalVisible: false,
    };
  },

  methods: {
    toggleModal() {
      this.isModalVisible = !this.isModalVisible;
    },
    handleSubmit(user) {
      this.users.push(user);
      this.saveToLocalStorage();
      this.toggleModal();
    },
    saveToLocalStorage() {
      localStorage.setItem("users", JSON.stringify(this.users));
    },
    loadFromLocalStorage() {
      const users = JSON.parse(localStorage.getItem("users"));
      if (users) {
        this.users = users;
      }
    },
  },
  mounted() {
    this.loadFromLocalStorage();
  },
};
</script>

<style scoped>
#app {
  font-family: "Arial", sans-serif;
  margin: 20px;
}

.header {
  margin-bottom: 20px;
}

.add-button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 4px;
  border: none;
  background-color: #1ac40ace;
  font-weight: 600;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-button:hover {
  background-color: #11b45a;
}
</style>
