<template>

  <div id="app">

    <new-student-form
        v-on:student-added="newStudentAdded"
    ></new-student-form>
    <!-- when "student-added" event is received from child, call newStudentAdded function -->

    <student-table
        v-bind:students="students"
        v-on:student-arrived-or-left="studentArrivedOrLeft"
        v-on:delete-student="studentDeleted"
    ></student-table>
    <!-- v-bind is used to bind students array in parent to students prop value in child -->

    <student-message
        v-bind:student="mostRecentStudent"
    ></student-message>

  </div>

</template>

<script>

import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {

  name: 'App',

  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },

  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },

  methods: {

    newStudentAdded(student) {
      this.students.push(student)
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
        /* return -1 if condition is true (s1 < s2, s1 earlier in alphabet) and return 1 if not true */
      })
    },

    studentArrivedOrLeft(student, present) {

      /* "find" method: loop through array, find array elements matching certain conditions */
      let updateStudent = this.students.find( (s) => {
        if (s.name === student.name && s.starID === student.starID) {
          return true
        }
      } )

      /* if a matching student is found, update "present" value of that student to the one passed from StudentTable/StudentRow */
      if (updateStudent) {
        updateStudent.present = present
        this.mostRecentStudent = updateStudent
      }
    },

    studentDeleted(student) {
      /* filter method: returns new array composed of all elements for which the function returns true */
      this.students = this.students.filter( (s) => {
        if (s != student) {return true}
      } )
      /* clear welcome/goodbye message */
      this.mostRecentStudent = {}
    }
  }
}

</script>

<style>

@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css"
/* styles are also applied to any child components - all 3 other components in this case */

</style>
