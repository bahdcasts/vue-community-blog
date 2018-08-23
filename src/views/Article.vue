<template>
  <h1 class="text-center">{{ $route.params.id }}</h1>
</template>

<script>
import Axios from 'axios';
import config from '@/config';

export default {
  mounted() {
    this.getArticle();
  },
  methods: {
    getArticle() {
      Axios.get(`${config.apiUrl}/article/${this.$route.params.id}`).then(response => {
        console.log(response);
      });
    }
  }
}
</script>

