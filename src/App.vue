<template>
  <div id="app">
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee"/>
    <employee-table :employees="employees" @delete:employee="deleteEmployee"/>
  </div>
</template>

<script>
  import EmployeeTable from './components/EmployeeTable.vue';
  import EmployeeForm from './components/EmployeeForm.vue';

  export default {
    name: 'App',
    components: {
      EmployeeForm,
      EmployeeTable,
    },
    data() {
      return {
        employees: [],
      };
    },

    mounted() {
      this.getEmployee();
    },
    methods: {
      async addEmployee(employee) {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          });
          const data = await response.json();
          this.employees = [...this.employees, data];
        } catch (e) {
          console.error(e);
        }

        // const lastId = this.employees.length > 0
        //   ? this.employees[this.employees.length - 1].id
        //   : 0;
        // const id = lastId + 1;
        // const newEmployee = {
        //   ...employee,
        //   id,
        // };
        // this.employees = [...this.employees, newEmployee];
      },
      async deleteEmployee(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'DELETE',
          });
          this.employees = this.employees.filter((e) => e.id !== id);
        } catch (e) {
          console.error(e);
        }
      },
      async editEmployee(id, updatedEmployee) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'PUT',
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json;charset=UTF-8' },
          });
          const data = await response.json();
          this.employees = this.employees.map(
            (employee) => (employee.id === id ? data : employee),
          );
        } catch (e) {
          console.error(e);
        }
      },
      async getEmployee() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          this.employees = await response.json();
        } catch (error) {
          console.error(error);
        }
      },

    },

  };

</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
