<template>
  <div>
    <h1>Create a new student:</h1>
    <form :disabled="!isFormValid" @submit.prevent="create">
      <b-input
        v-model.trim="username"
        :state="isUsernameValid"
        required
        placeholder="Enter your username"
      />
      <b-input
        v-model.trim="password"
        :state="isPasswordValid"
        required
        placeholder="Enter your password"
      />
      <b-input
        v-model.trim="name"
        :state="isNameValid"
        required
        placeholder="Enter your name"
      />
      <b-input
        ref="email"
        v-model.trim="email"
        type="email"
        :state="isEmailValid"
        required
        pattern=".+@my.ipleiria.pt"
        placeholder="Enter your e-mail"
      />
      <b-select
        v-model="courseCode"
        :options="courses"
        :state="isCourseValid"
        required
        value-field="code"
        text-field="name"
      >
        <template v-slot:first>
          <option :value="null" disabled>-- Please select the Course --</option>
        </template>
      </b-select>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/students">Return</nuxt-link>
      <button type="reset" @click="reset">RESET</button>
      <button @click.prevent="create" :disabled="!isFormValid">CREATE</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: null,
      password: null,
      name: null,
      email: null,
      courseCode: null,
      courses: [],
      errorMsg: false,
    };
  },
  created() {
    this.$axios.$get("/api/courses").then((courses) => {
      this.courses = courses;
    });
  },
  computed: {
    isUsernameValid() {
      if (!this.username) {
        return null;
      }

      let usernameLen = this.username.length;
      if (usernameLen < 3 || usernameLen > 15) {
        return false
      }

      return true
    },

    isPasswordValid() {
      if (!this.password) {
        return null
      }

      let passwordLen = this.password.length;
      if (passwordLen < 3 || passwordLen > 255) {
        return false
      }

      return true
    },

    isNameValid() {
      if (!this.name) {
        return null
      }
      let nameLen = this.name.length;
      if (nameLen < 3 || nameLen > 25) {
        return false
      }
      return true
    },
    isEmailValid() {
      if (!this.email) {
        return null
      }

      return this.$refs.email.checkValidity()
    },
    isCourseValid() {
      if (!this.courseCode) {
        return null
      }
      return this.courses.some((course) => this.courseCode === course.code)
    },
    isFormValid () {
      if (!this.isUsernameValid) {
        return false
      }
      if (!this.isPasswordValid) {
        return false
      }
      if (!this.isNameValid) {
        return false
      }
      if (!this.isEmailValid) {
        return false
      }
      if (!this.isCourseValid) {
        return false
      }
      return true
    }
  },
  methods: {
    reset() {
      this.errorMsg = false
    },
    create() {
      this.$axios
        .$post("/api/students", {
          username: this.username,
          password: this.password,
          name: this.name,
          email: this.email,
          courseCode: this.courseCode
        })
        .then(() => {
          this.$router.push("/");
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  }

}
</script>
