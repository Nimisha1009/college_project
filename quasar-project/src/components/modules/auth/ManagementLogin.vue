<template>

  <q-page class="bg-green   row justify-center items-center">
    <div class="column bordered">
      <div class="row flex flex-center">
        <h5 class="text-h5 text-white q-my-md">Admin Login</h5>
      </div>
      <div class="row">
        <q-card square bordered class="q-pa-lg shadow-1">
          <q-card-section>
            <q-form class="q-gutter-md">
              <q-input square filled clearable v-model="auth.email" label="Email" />
              <q-input square filled clearable v-model="auth.password" type="password" label="password" />

            </q-form>
          </q-card-section>
          <q-card-actions class="q-px-md">
            <q-btn unelevated :color="authSuccess ? 'green' : 'primary'" size="lg" class="full-width"
              :label="authSuccess ? 'success' : 'login'" @click="login" :disable="authInProgress || authSuccess"
              :loading="authInProgress" />
          </q-card-actions>

        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      auth: {
        interface: 'management'
      },
      authInProgress: false,
      authsuccess: false,
    }
  },
  methods: {
    async login () {
      this.$api.defaults.headers.common['Authorization'] = null;
      this.authInProgress = true
      try {
        let httpRequest = await this.$axios.post('http://localhost:8055/auth/login', this.auth)
        this.authSuccess = true
        this.authInProgress = false
        console.log(httpRequest)
        let access_token = httpRequest.data.data.access_token
        this.$axios.defaults.headers.common['Authorization'] = 'Bearer ' + access_token;
        localStorage.setItem('access_token', access_token)
        this.$mitt.emit('login-successfull')
        setTimeout(() => {
          this.$router.replace('/management/Dashboard')
        }, 1000)
      } catch (err) {
        alert('Some Error Occurred')
        this.authInProgress = false
      }

    },
  }
}



</script>
