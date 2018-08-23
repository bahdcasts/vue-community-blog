<template>
  <div class="row">
    <div class="col-md-8 offset-md-2" v-if="articles.data" v-for="article in articles.data" :key="article.id">
      <Article :article="article" />
    </div>
  </div>
</template>


<script>
import Axios from "axios";
import config from "@/config";
import Article from "@/components/Article.vue";

export default {
  components: {
    Article
  },
  mounted() {
    this.getArticles();
  },
  data() {
    return {
      articles: {}
    };
  },
  methods: {
    getArticles() {
      Axios.get(`${config.apiUrl}/articles`).then(response => {
        this.articles = response.data.data;
      });
    }
  }
};
</script>
