<template>
  <div>

    <div class="card student-list m-2 p-2">

      <h4 class="card-title">Student List</h4>

      <div class="edit-table-toggle form-check">
        <input id="edit-table" type="checkbox" class="form-check-input" v-model="editTable">
        <label for="edit-table" class="form-check-label">Edit table?</label>
      </div>

      <div id="student-table">
        <table class="table">

          <tr>
            <th>Name</th>
            <th>StarID</th>
            <th>Present?</th>
            <th v-show="editTable">Delete</th>
          </tr>

          <!-- Considered bad practice to update the value of a prop from within the same child component where it's
          defined. Want to have only one source from which a prop can be updated to avoid conflicts. Should not update
          the "present" value from within the StudentTable component. Instead emit event to App.vue when checkbox is
          checked/unchecked and App.vue will update data. "Data down, events up" -->

          <student-row
              v-for="student in students"
              v-bind:student="student"
              v-bind:key="student.starID"
              v-bind:edit="editTable"
              v-on:student-arrived-or-left="arrivedOrLeft"
              v-on:delete-student="deleteStudent"
          ></student-row>

        </table>
      </div>

    </div>

  </div>
</template>

<script>

import StudentRow from '@/components/StudentRow.vue'

export default {

  name: 'StudentTable',

  components: {
    StudentRow
  },

  emits: ['student-arrived-or-left'],

  props: {
    students: Array,
  },

  data() {
    return {
      editTable: false
    }
  },

  methods: {
    arrivedOrLeft(student, present) {
      this.$emit('student-arrived-or-left', student, present)
    },
    deleteStudent(student) {
      if (confirm(`Delete ${this.student.name}?`)) {
        this.$emit('delete-student', student)
      }
    }
  }

}

</script>

<style scoped>




</style>