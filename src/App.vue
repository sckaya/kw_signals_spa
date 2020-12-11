<template>
  <div class="vue-tempalte ">
    <!-- Navigation -->
    <nav class="navbar justify-content-between flex-row static-top">
        <h2>Product Viewer</h2>
        <ul class="nav navbar-nav flex-row float-right">
          <li class="nav-item" v-if="!isLoggedIn">
            <router-link class="btn btn-outline-primary" to="/login">Log in</router-link>
          </li>
          <li class="nav-item" v-if="!isLoggedIn">
            <router-link class="btn btn-outline-primary" to="/register">Register</router-link>
          </li>
            <li class="nav-item" v-if="isLoggedIn">
              <a class="btn btn-outline-primary" @click="logout">Logout</a>
            </li>
        </ul>
    </nav>

    <!-- Main -->
    <div class="App">
        <div class="flex-row">
          <router-view />
        </div>
    </div>
  </div>
</template>
<script>
  export default {
    computed : {
      isLoggedIn : function(){ return this.$store.getters.isLoggedIn}
    },
    methods: {
      logout: function () {
        this.$store.dispatch('logout')
        .then(() => {
          this.$router.push('/login')
        })
      }
    },
    created: function () {
      this.$http.interceptors.response.use(undefined, function (err) {
        return new Promise(function (resolve, reject) {
          if (err.status === 401 && err.config && !err.config.__isRetryRequest) {
            this.$store.dispatch(logout)
          }
          throw err;
        });
      });
    }
  }
</script>