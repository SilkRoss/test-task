<template>
  <div class="table-container">
    <table class="user-table">
      <thead>
        <tr>
          <th @click="sort('name')" class="name-column">
            Имя
            <span v-if="sortKey === 'name'">{{ sortAsc ? "▲" : "▼" }}</span>
          </th>
          <th @click="sort('phone')" class="phone-column">
            Телефон
            <span v-if="sortKey === 'phone'">{{ sortAsc ? "▲" : "▼" }}</span>
          </th>
        </tr>
      </thead>

      <UserRow v-for="user in sortedUsers" :key="user.id" :user="user" />
    </table>
  </div>
</template>

<script>
import _ from "lodash";
import UserRow from "./UserRow.vue";

export default {
  components: {
    UserRow,
  },
  props: ["users"],
  data() {
    return {
      sortKey: "",
      sortAsc: true,
    };
  },
  computed: {
    sortedUsers() {
      if (!this.sortKey) return this.users;
      return this.sortUsers(this.users);
    },
  },
  methods: {
    sort(key) {
      if (this.sortKey === key) {
        this.sortAsc = !this.sortAsc;
      } else {
        this.sortKey = key;
        this.sortAsc = true;
      }
    },
    sortUsers(users) {
      return users
        .map((user) => {
          if (user.children && user.children.length) {
            user.children = this.sortUsers(user.children);
          }
          return user;
        })
        .sort((a, b) => {
          if (this.sortAsc) {
            return a[this.sortKey] > b[this.sortKey] ? 1 : -1;
          } else {
            return a[this.sortKey] < b[this.sortKey] ? 1 : -1;
          }
        });
    },
  },
};
</script>

<style scoped>
.table-container {
  width: 100%;
  overflow-x: auto;
}

.user-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  table-layout: fixed;
}

.user-table th,
.user-table td {
  border: 1px solid #000000;
  padding: 12px;
  text-align: left;
  overflow: hidden;
  text-overflow: ellipsis;
}

.user-table th {
  background-color: #f5f5f5;
  cursor: pointer;
}
</style>
