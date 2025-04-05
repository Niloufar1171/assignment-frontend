<template>
  <form @submit.prevent="saveEmployee">
    <input v-model="employee.firstName" placeholder="First Name" required />
    <input v-model="employee.lastName" placeholder="Last Name" required />
    <input v-model="employee.salary" type="number" placeholder="Salary" required />
    <button type="submit">{{ employee.id ? 'Update' : 'Add' }} Employee</button>
  </form>
</template>

<script>
export default {
  props: ['employee'],
  methods: {
    saveEmployee() {
      this.$emit('save', this.employee);  // Emit the save event to parent with employee data
    }
  }
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
input {
  margin-bottom: 10px;
}
button {
  width: 100%;
}
</style>
