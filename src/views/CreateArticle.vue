<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div class="card my-5">
          <div class="card-body">
            <input type="text" placeholder="Title" class="mb-3 form-control">
            <wysiwyg v-model="content" />
            <div class="text-center">
              <button class="btn-success btn btn-lg mt-3">Create Article</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      content: ""
    };
  }
};
</script>
