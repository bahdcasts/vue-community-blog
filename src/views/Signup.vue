<template>
  <div class="row my-5">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-body">
          <h3 class="text-center my-4">Signup</h3>
          <div class="form-group">
            <input v-model="name" type="text" placeholder="Name" class="form-control">
          </div>
          <div class="form-group">
            <input v-model="email" type="text" placeholder="Email" class="form-control">
          </div>
          <div class="form-group">
            <input v-model="password" type="password" placeholder="Password" class="form-control">
          </div>
          <div class="form-group text-center">
            <button @click="registerUser()" class="btn form-control btn-success">Signup</button>
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
      name: "",
      email: "",
      password: ""
    };
  },
  methods: {
    registerUser() {
      Axios.post("https://react-blog-api.bahdcasts.com/api/auth/register", {
        name: this.name,
        email: this.email,
        password: this.password
      })
        .then((response) => {
          const { data } = response.data;
          localStorage.setItem('auth', JSON.stringify(data))
          this.$root.auth = data;

          this.$router.push('home');
        })
        .catch(({ response }) => {
          console.log(response);
        });
    }
  }
};
</script>
