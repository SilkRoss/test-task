<template>
  <tbody>
    <tr :style="{ backgroundColor: user.backgroundColor || 'transparent' }">
      <td class="name-column">{{ user.name }}</td>
      <td class="phone-column">{{ user.phone }}</td>
    </tr>
    <tr v-if="user.children && user.children.length">
      <td colspan="2">
        <div class="table-container">
          <table class="user-table">
            <UserRow
              v-for="child in user.children"
              :key="child.id"
              :user="child"
            />
          </table>
        </div>
      </td>
    </tr>
  </tbody>
</template>

<script>
export default {
  name: "UserRow",
  props: ["user"],
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
  margin-top: 10px;
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
