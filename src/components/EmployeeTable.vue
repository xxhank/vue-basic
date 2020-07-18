<template>
  <div id="employee-table">
    <p v-if="employees.length<1" class="empty-table">
      No employees
    </p>
    <table v-else>
      <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="employee in employees" :key="employee.id">
        <td v-if="editing===employee.id">
          <input type="text" v-model="employee.name">
        </td>
        <td v-else>{{employee.name}}</td>

        <td v-if="editing=== employee.id">
          <input type="text" v-model="employee.email">
        </td>
        <td v-else>{{employee.email}}</td>

        <td v-if="editing === employee.id">
          <button @click="editEmployee(employee)">Save</button>
          <button @click="cancelEdit(employee)" class="muted-button">Cancel</button>
        </td>
        <td v-else>
          <button @click="editMode(employee)">Edit</button>
          <button @click="$emit('delete:employee', employee.id)">Delete</button>
        </td>
      </tr>

      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'employee-table',
    props: {
      employees: Array,
    },
    data() {
      return {
        editing: null,
      };
    },
    methods: {
      editMode(employee) {
        this.cacheEmployee = { ...employee };
        this.editing = employee.id;
      },
      cancelEdit(employee) {
        Object.assign(employee, this.cacheEmployee);
        this.editing = null;
      },
      editEmployee(employee) {
        if (employee.nabla === '' || employee.email === '') return;
        this.$emit('edit:employee', employee.id, employee);
        this.editing = null;
      },
    },
  };
</script>

<style scoped>
  /*button {*/
  /*  background: #009435;*/
  /*  border: 1px solid #009435;*/
  /*}*/
  button {
    margin: 0 0.5rem 0 0;
  }

  .small-container {
    max-width: 680px;
  }
</style>
