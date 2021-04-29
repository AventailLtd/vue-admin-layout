<template>
  <div class="row">
    <div class="col-12">
      <div class="login-form-wrapper d-flex align-items-center justify-content-center">
        <div class="login-form">
          <div class="page-header mb-5 text-center">
            <h2>Bejelentkezés</h2>
          </div>
          <div class="text-center">
            <form id="login-form" @submit.prevent="login">
              <div class="form-group">
                <label for="username">Felhasználó:</label>
                <input id="username"
                  v-model="username"
                  type="text"
                  class="form-control"
                  placeholder="felhasználónév"
                  name="username"
                  :disabled="loginInProgress"
                >
              </div>
              <div class="form-group">
                <label for="password">Jelszó:</label>
                <input id="password"
                  v-model="password"
                  type="password"
                  class="form-control"
                  placeholder="********"
                  name="password"
                  :disabled="loginInProgress"
                >
              </div>
              <input type="submit"
                class="btn btn-primary"
                value="Belépés"
                :disabled="loginInProgress"
              >
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

/**
 * A beimportált libet nem lássa a vue.
 * https://youtrack.jetbrains.com/issue/WEB-40356?_ga=2.108837760.349915712.1618300833-1564400358.1617894368
 *
 * @property $dialog
 */
export default {
  name: 'Login',
  data() {
    return {
      loginInProgress: false,
      username: '',
      password: '',
    }
  },
  methods: {
    login() {
      let errors = false
      if (this.username.trim() === '') {
        console.log('felhasználónév üres!')
        errors = true
      }
      if (this.password === '') {
        console.log('jelszó üres!')
        errors = true
      }

      if (errors) {
        this.$root.$emit('showMessageFailure', 'Minden mezőt ki kell tölteni!')
        return
      }

      this.loginInProgress = true
      this.$store.dispatch('Auth/login', { username: this.username, password: this.password })
        .then(() => {
          if (this.$store.getters['Auth/isLoggedIn']) {
            let route = '/'
            if (this.$route.query.redirect !== undefined) {
              route = this.$route.query.redirect.toString()
            }
            this.$root.$emit('showMessageSuccess', 'Sikeres bejelentkezés!')
            this.$router.replace(route)
          } else {
            this.$root.$emit('showMessageFailure', 'Érvénytelen belépési adatok!')
          }
          this.loginInProgress = false
        })
    },
  },
}
</script>

<style scoped lang="scss" src="./Login.scss" />
