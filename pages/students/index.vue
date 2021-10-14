<template>
  <b-container>
    <b-table striped over :items="students" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/students/${row.item.username}`">Details</nuxt-link>
      </template>
    </b-table>
    <nuxt-link to="/create">Create a New Student</nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      fields: ['username', 'name', 'email', 'courseName', 'actions'],
      students: []
    }
  },
  created () {
    this.$axios.$get('/api/students')
      .then((students) => {
        this.students = students
      })
  }
}
</script>
