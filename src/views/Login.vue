<template>
  <div class="row my-5">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-body">
          <h3 class="text-center my-4">Login</h3>
          <div class="form-group">
            <input v-bind:class="{ 'is-invalid': errors.email }" v-model="email" type="text" placeholder="Email" class="form-control">
            <div class="errors" v-if="errors.email">
              <small class="text-danger" :key="error" v-for="error in errors.email">{{ error }}</small>
            </div>
          </div>
          <div class="form-group">
            <input v-bind:class="{ 'is-invalid': errors.password }" v-model="password" type="password" placeholder="Password" class="form-control">
            <div class="errors" v-if="errors.password">
              <small class="text-danger" :key="error" v-for="error in errors.password">{{ error }}</small>
            </div>
          </div>
          <div class="form-group text-center">
            <button @click="loginUser()" class="btn form-control btn-success">Login</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";

export default {
  data() {
    return {
      email: "",
      password: "",
      errors: {}
    };
  },
  methods: {
    loginUser() {
      Axios.post("https://react-blog-api.bahdcasts.com/api/auth/login", {
        email: this.email,
        password: this.password
      })
        .then(response => {
          this.$root.auth = response.data.data;
          localStorage.setItem("auth", JSON.stringify(response.data.data));
          this.$router.push("home");
        })
        .catch(({ response }) => {
          if (response.status === 401) {
            this.errors = {
              email: ["These credentials do not match our records."]
            };
          } else {
            this.errors = response.data;
          }
        });
    }
  }
};
</script>
