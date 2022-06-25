<template>
  <v-container>
    <v-card style="padding: 40px">
      <v-text-field
        placeholder="نام کاربری"
        label="نام کاربری"
        v-model="username"
      ></v-text-field>
      <v-text-field
        placeholder="رمز عبور"
        label="رمز عبور"
        type="password"
        v-model="password"
      ></v-text-field>
      <v-btn
        class="primary"
        @click="userLogin"
      >
        ورود
      </v-btn>
    </v-card>
  </v-container>
</template>

<script>
export default {
  auth: false,
  name: "login",
  data() {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    async userLogin() {
      try {
        let response = await this.$auth.loginWith('local', {
          data:
            {username: this.username, password: this.password}
        })
        console.log(response)
        if (response.data.status)
          window.location.href = '/new_qa';
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style scoped>

</style>
