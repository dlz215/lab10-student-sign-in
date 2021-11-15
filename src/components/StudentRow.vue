<template>
  <tr v-bind:class="{present: student.present, absent: !student.present}">
    <td>{{ student.name }}</td>
    <td>{{ student.starID }}</td>
    <td>
      <input type="checkbox" v-bind:checked="student.present" v-on:change="arrivedOrLeft(student, $event.target.checked)">
      <!-- v-bind used to bind the "checked" property to the "present" value of each student in array. If "student.present" is true,
      box has checked property -->
      <!-- "$event.checked" is element generating event (checkbox). "checked" is property of that element being examined.
      Checking box passes student data and checked/unchecked boolean to method. -->
    </td>
    <td v-show="edit"><img v-on:click="deleteStudent" src="@/assets/delete.png"></td>
  </tr>
</template>

<script>

export default {
  name: 'StudentRow',
  props: {
    student: Object,
    edit: Boolean
  },
  methods: {
    arrivedOrLeft(student, present) {
      this.$emit('student-arrived-or-left', student, present)
    },
    deleteStudent() {
      this.$emit('delete-student', this.student)
    }
  }
}

</script>

<style scoped>

.present {
  color: gray;
  font-style: italic;
}

.absent {
  color: black;
  font-weight: bold;
}

</style>