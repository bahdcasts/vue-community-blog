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
            <input type="text" placeholder="Title" class="mb-3 form-control">
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
import Axios from 'axios';
import PictureInput from "vue-picture-input";

export default {
  components: {
    PictureInput
  },
  data() {
    return {
      content: "",
      image: null
    };
  },
  methods: {
    onChange(image) {
      this.image = image;
    },
    createArticle() {
      const form = new FormData();
      form.append('file', this.image);
      form.append('upload_preset', 'g5ziunzg');
      form.append('api_key', '132255634713478');

      Axios.post('https://api.cloudinary.com/v1_1/bahdcoder/image/upload', form)
        .then(res => console.log(res));
    }
  }
};
</script>
