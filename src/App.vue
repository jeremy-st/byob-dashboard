<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <v-icon size="30px" color="primary">mdi-emoticon</v-icon>
        &nbsp;
        <span class="font-weight-light">Bring Your Own Brand</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <router-link
          tag='button' id="login-button"
          to="/login"
          v-if="!loginRedirect && !authenticated"
        >
        <v-btn text>Login</v-btn>
      </router-link>
      <div v-if="authenticated">
        <!-- <router-link
            tag='button' id='home-button'
            v-if="authenticated"
            to="/"
          > -->
          <v-btn text @click="home">
            <v-icon left dark>mdi-home</v-icon>
            Home
          </v-btn>
        <!-- </router-link> -->

        <ProfileButton
          v-bind:userinfo="userinfo"
          >
        </ProfileButton>
      </div>
    </v-app-bar>
    <v-content>    
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>
import ProfileButton from '@/components/ProfileButton'

export default {
  name: 'App',
  data () {
    return {
      loginRedirect: false,
      authenticated: false,
      userinfo: undefined,
      key: 0
    }
  },
  components: {
      ProfileButton
  },
  created () {
    this.appInit()
  },
  watch: {
    // Everytime the route changes, check for auth status
    '$route': 'isAuthenticated'
  },
  methods: {
    appInit() {
      if (this.$config.oidc.redirect_uri)
        this.loginRedirect = this.$config.oidc.redirect_uri
      this.isAuthenticated()
    },
    async isAuthenticated () {
      this.authenticated = await this.$auth.isAuthenticated()

      if (!this.userinfo) {
        // hacky-hack. Since we're directly updating userinfo. Do not fetch new data because there may be a race-condition and data from Okta could lag the UI
        this.userinfo = await this.$auth.getUser()
      }
    },
    home() {
      this.$router.push({
          name: 'home',
      })
    }
  }
}
</script>