<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div class="card my-5">
          <div class="card-body">
            <picture-input
              accept="image/jpeg,image/png" 
              size="5"
              button-class="btn btn-danger"
              @change="onChange" />
            <select class="form-control my-3" v-model="category">
              <option selected>Select a Category</option>
              <option :value="category.id" :key="category.id" v-for="category in categories">
                {{ category.name }}
              </option>
            </select>
            <input type="text" placeholder="Title" v-model="title" class="my-3 form-control">
            <wysiwyg v-model="content" />
            <div class="text-center">
              <button :disabled="loading" @click="createArticle()" class="btn-success btn mt-3">
                <i class="fas fa-spin fa-spinner" v-if="loading"></i>
                {{ loading ? '' : 'Create Category' }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
import config from "@/config";
import PictureInput from "vue-picture-input";

export default {
  beforeRouteEnter(to, from, next) {
    if (!localStorage.getItem("auth")) {
      return next({ path: "/login" });
    }

    next();
  },
  mounted() {
    this.getCategories();
  },
  components: {
    PictureInput
  },
  data() {
    return {
      title: "",
      content: "",
      image: null,
      categories: [],
      category: "",
      loading: false
    };
  },
  methods: {
    onChange(image) {
      this.image = image;
    },
    createArticle() {
      if (!this.title || !this.image || !this.category || !this.content) {
        this.$noty.error("Please fill in all fields.");
        return;
      }

      this.loading = true;
      const form = new FormData();
      form.append("file", this.image);
      form.append("upload_preset", process.env.VUE_APP_CLOUDINARY_PRESET);
      form.append("api_key", process.env.VUE_APP_CLOUDINARY_API_KEY);

      Axios.post(process.env.VUE_APP_CLOUDINARY_URL, form)
        .then(res =>
          Axios.post(
            `${config.apiUrl}/articles`,
            {
              title: this.title,
              content: this.content,
              category_id: this.category,
              imageUrl: res.data.secure_url
            },
            {
              headers: {
                Authorization: `Bearer ${this.$root.auth.token}`
              }
            }
          )
            .then(() => {
              this.loading = false;
              this.$noty.success("Article created successfully.");
              this.$router.push("/");
            })
            .catch(() => {
              this.loading = false;
              this.$noty.error("Oops ! Something went wrong.");
            })
        )
        .catch(() => {
          this.loading = false;
          this.$noty.error("Oops ! Something went wrong.");
        });
    },
    getCategories() {
      const categories = localStorage.getItem("categories");

      if (categories) {
        this.categories = JSON.parse(categories);
        return;
      }
      Axios.get(`${config.apiUrl}/categories`).then(res => {
        this.categories = res.data.categories;
        localStorage.setItem("categories", JSON.stringify(this.categories));
      });
    }
  }
};
</script>
