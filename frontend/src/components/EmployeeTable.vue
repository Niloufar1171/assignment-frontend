<template>
  <table v-if="employees.length">
    <thead>
      <tr>
        <th>ID</th>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Salary</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="emp in employees" :key="emp.id">
        <td>{{ emp.id }}</td>
        <td>{{ emp.firstName }}</td>
        <td>{{ emp.lastName }}</td>
        <td>{{ emp.salary }}</td>
        <td>
          <button @click="$emit('edit', emp)">Edit</button>
          <button @click="$emit('delete', emp.id)">Delete</button>
        </td>
      </tr>
    </tbody>
  </table>
  <p v-else>No employees found.</p>
</template>

<script>
export default {
  props: ['employees']
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  border: 1px solid #ddd;
  padding: 8px;
}
th {
  background-color: #f2f2f2;
}
button {
  margin: 5px;
}
</style>
