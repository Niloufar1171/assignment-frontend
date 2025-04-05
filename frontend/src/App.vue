<template>
  <div>
    <h1>Employee Managements for all CRUDS</h1>

    <!-- Search Bar -->
    <input 
      v-model="searchQuery" 
      type="text" 
      placeholder="Search Employee" 
      @input="searchEmployee"
    />

    <!-- Employee Form for adding/updating employee -->
    <EmployeeForm :employee="employee" @save="saveEmployee" />
    
    <!-- Employee Table displaying list of employees -->
    <EmployeeTable 
      :employees="filteredEmployees" 
      @edit="editEmployee" 
      @delete="deleteEmployee"
    />
    
    <!-- Button to load more employees -->
    <button @click="loadMoreEmployees" v-if="employees.length > 10">Load More</button>
  </div>
</template>

<script>
import axios from 'axios';
import EmployeeForm from './components/EmployeeForm.vue';
import EmployeeTable from './components/EmployeeTable.vue';

export default {
  components: { EmployeeForm, EmployeeTable },
  data() {
    return {
      employees: [],
      employee: {
        firstName: '',
        lastName: '',
        salary: ''
      },
      searchQuery: '',
      page: 0,
      limit: 10,
    };
  },
  created() {
    this.fetchEmployees();
  },
  computed: {
    // Filtered employees based on search query
    filteredEmployees() {
      if (this.searchQuery) {
        return this.employees.filter(emp => 
          emp.firstName.toLowerCase().includes(this.searchQuery.toLowerCase()) || 
          emp.lastName.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      }
      return this.employees.slice(0, this.limit);  // Only show first 10 employees
    }
  },
  methods: {
    fetchEmployees() {
      axios.get(`http://localhost:8080/api/employees?page=${this.page}&size=${this.limit}`)
        .then(res => {
          this.employees = res.data;
        });
    },
    saveEmployee(emp) {
      if (emp.id) {
        axios.put(`http://localhost:8080/api/employees/${emp.id}`, emp)
          .then(() => {
            this.fetchEmployees();
            this.employee = { firstName: '', lastName: '', salary: '' };
          });
      } else {
        axios.post('http://localhost:8080/api/employees', emp)
          .then(() => {
            this.fetchEmployees();
            this.employee = { firstName: '', lastName: '', salary: '' };
          });
      }
    },
    editEmployee(emp) {
      this.employee = { ...emp };  // Copy the employee data to the form for editing
    },
    deleteEmployee(id) {
      axios.delete(`http://localhost:8080/api/employees/${id}`)
        .then(() => {
          this.fetchEmployees();
        });
    },
    loadMoreEmployees() {
      this.page += 1;
      this.fetchEmployees();
    },
    searchEmployee() {
      // Trigger search when user types in the search bar
      this.page = 0;  // Reset to first page when searching
      this.fetchEmployees();
    }
  }
};
</script>
