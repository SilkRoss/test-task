<template>
  <div id="app">
    <div class="header">
      <button class="add-button" @click="toggleModal">Добавить</button>
    </div>
    <UserTable :users="users" />
    <Modal :isVisible="isModalVisible" @close="toggleModal">
      <UserForm :users="getAllUsers(users)" @submit="handleSubmit" />
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
        {
          id: 1,
          name: "MIKE TYSON",
          phone: "+88005553535",
          children: [],
          backgroundColor: "#08e9788a",
        },
        {
          id: 2,
          name: "Эльвира Набиуллина",
          phone: "+181818181818",
          children: [],
          backgroundColor: "#ee3c068a",
        },
      ],
      isModalVisible: false,
    };
  },
  methods: {
    toggleModal() {
      this.isModalVisible = !this.isModalVisible;
    },
    handleSubmit(user) {
      if (user.parent === null) {
        user.backgroundColor = this.getRandomColor();
      }
      this.addUser(this.users, user);
      this.saveToLocalStorage();
      this.toggleModal();
    },
    addUser(users, user) {
      if (user.parent === null) {
        users.push(user);
        return;
      }

      for (let i = 0; i < users.length; i++) {
        if (users[i].id === user.parent) {
          if (!users[i].children) {
            this.$set(users[i], "children", []);
          }
          users[i].children.push(user);
          return;
        } else if (users[i].children) {
          this.addUser(users[i].children, user);
        }
      }
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
    getAllUsers(users) {
      let allUsers = [];
      const traverse = (user) => {
        allUsers.push(user);
        if (user.children) {
          user.children.forEach(traverse);
        }
      };
      users.forEach(traverse);
      return allUsers;
    },
    getRandomColor() {
      const letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
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
