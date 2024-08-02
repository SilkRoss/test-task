<template>
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
    <tbody>
      <tr v-for="user in sortedUsers" :key="user.id">
        <td class="name-column">{{ user.name }}</td>
        <td class="phone-column">{{ user.phone }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import _ from "lodash";

export default {
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
      return _.orderBy(
        this.users,
        [this.sortKey],
        [this.sortAsc ? "asc" : "desc"]
      );
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
  },
};
</script>

<style scoped>
.user-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  table-layout: fixed;
}

.user-table th,
.user-table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
  overflow: hidden;
  text-overflow: ellipsis;
}

.user-table th {
  background-color: #f5f5f5;
  cursor: pointer;
}

.user-table tr:hover {
  background-color: #f1f1f1;
}

.name-column {
  width: 50%;
}

.phone-column {
  width: 50%;
}
</style>
