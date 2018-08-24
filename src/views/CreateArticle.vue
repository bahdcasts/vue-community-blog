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
            <input type="text" placeholder="Title" class="my-3 form-control">
            <wysiwyg v-model="content" />
            <div class="text-center">
              <button @click="createArticle()" class="btn-success btn btn-lg mt-3">Create Article</button>
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
  mounted() {
    this.getCategories();
  },
  components: {
    PictureInput
  },
  data() {
    return {
      content: "",
      image: null,
      categories: [],
      category: ""
    };
  },
  methods: {
    onChange(image) {
      this.image = image;
    },
    createArticle() {
      const form = new FormData();
      form.append("file", this.image);
      form.append("upload_preset", process.env.VUE_APP_CLOUDINARY_PRESET);
      form.append("api_key", process.env.VUE_APP_CLOUDINARY_API_KEY);

      Axios.post(process.env.VUE_APP_CLOUDINARY_URL, form).then(res =>
        console.log(res)
      );
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
