<template>
  <div class="app flex-row align-items-center login-body" >
    <div class="container">
      <b-row class="justify-content-center">
        <b-col md="8">
          <b-card-group>
            <b-card no-body class="p-4">
              <b-card-body>
                <b-form id="login-form" @submit="login">
                  <h1>Login</h1>
                  <p class="text-muted">Sign In to your account</p>
                  <b-input-group class="mb-3">
                    <b-input-group-prepend>
                      <b-input-group-text>
                        <i class="icon-user"></i>
                      </b-input-group-text>
                    </b-input-group-prepend>
                    <b-form-input
                      type="text"
                      v-model="username"
                      class="form-control"
                      placeholder="Username"
                      autocomplete="username email"
                    />
                  </b-input-group>
                  <b-input-group class="mb-4">
                    <b-input-group-prepend>
                      <b-input-group-text>
                        <i class="icon-lock"></i>
                      </b-input-group-text>
                    </b-input-group-prepend>
                    <b-form-input
                      type="password"
                      v-model="password"
                      class="form-control"
                      placeholder="Password"
                      autocomplete="current-password"
                    />
                  </b-input-group>
                  <b-row>
                    <b-col cols="6">
                      <b-button type="submit" variant="primary" class="px-4">Login</b-button>
                    </b-col>
                    <b-col cols="6" class="text-right">
                      <b-button variant="link" class="px-0">Forgot password?</b-button>
                    </b-col>
                  </b-row>
                  <div style="margin-top:10px;">
                      <b-alert :show="is_error" variant="danger">{{error_msg}}</b-alert>
                  </div>
                </b-form>
              </b-card-body>
            </b-card>
            <b-card no-body class="text-white bg-primary py-5 d-md-down-none" style="width:44%">
              <b-card-body class="text-center">
                <div>
                  <h2>Sign up</h2>
                  <p>Create a new user here.</p>
                  <b-button
                    v-on:click="register()"
                    variant="primary"
                    class="active mt-3"
                  >Add User!</b-button>
                  
                </div>
              </b-card-body>
            </b-card>
          </b-card-group>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',
  data () {
    return {
      username: null,
      password: null,
      error_msg: "",
      is_error: false,
    }
  },
  methods: {
    login: function(event) {
      let vm = this;
      let payload = {
        email: this.username,
        password: this.password,
      }
      axios.post('/users/login', payload).then(function (resp) {
        if (resp.status  == 200) {
          vm.$store.dispatch('storeToken', resp.data.token);
          vm.$store.dispatch('storeUser', resp.data);
          vm.$router.push({ name: 'Dashboard' });
        }
        else
        {
          // console.log(resp.status);
          // console.log(resp.data.token);
          // console.log(resp.data);
          vm.is_error = true;
          vm.error_msg = resp.status;
        }
      })
    },
    register: function(event) {
      let vm = this;
      vm.$router.push({ name: 'Register' });
    }
  }
};
</script>

<style>
  /* IE fix */
  .login-body {
    background-position: center top;
    background-repeat: no-repeat;
    width: 100%;
    background-attachment: fixed;
    background-size: cover;
  }
</style>
