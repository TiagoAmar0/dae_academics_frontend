<template>
  <b-container>
    <b-table striped over :items="students" :fields="fields" />
    <nuxt-link to="/create">Create a New Student</nuxt-link>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      fields: ['username', 'name', 'email'],
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
