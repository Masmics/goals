<template>
  <div id="app">
    <header>
      <img alt="goals logo" src="../assets/goals.jpg">
      <span v-if="user">
        Hello, {{ user.username }}!
      </span>
      <nav v-if="user">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink t="/auth">Sign Up/Sign In</RouterLink>
        <RouterLink to="/goals">Goals</RouterLink>
        <a href="#" @click="handleLogout">Logout</a>
      </nav>
    </header>

    <main>
      <RouterView v-if="user" :user="user"/>
      <Auth v-else
        :onSignUp="handleSignup"
        :onSignIn="handleSignIn"
      />
    </main>
  </div>
</template>

<script>
import api from '../services.api';
import Auth from './auth/Auth';

export default {
  data() {
    return{
      user: null
    };
  },
  components: {
    Auth
  },
  created() {
    const json = window.localStorage.getItem('profile');
    if(json) {
      this.setUser(JSON.parse(json));
    }
  },
  methods: {
    handleSignUp(profile) {
      return api.signUp(profile)
        .then(user => {
          this.setUser(user);
        });
    },
    handleSignIn(credentials) {
      return api.signIn(credentials)
        .then(user => {
          this.setUser(user);
        });
    },
    setUser(user) {
      this.user = user;
      if(user) {
        api.setToken(user.id)
        window.localStorage.setItem('profile', JSON.stringify(user));       
      }
      else {
        api.setToken();
        window.localStorage.removeItem('profile');
      }
    },
    handleLogout() {
      this.setUser(null);
      this.$router.push('/');
    }
  }
};
</script>

<style scoped>
header {
  height: 80px;
  background: grey;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
header img {
  height: 75%;
}
nav a {
  color: black;
  margin: 4px;
  padding: 4px;
  border: 1px solid black;
}
main {
  padding: 10px;
}

</style>
