<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>EDIT POST</h4>
            <hr />

            <form @submit.prevent="update">
              <div class="form-group">
                <label for="title" class="font-weight-bold">TITLE</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="post.title"
                  placeholder="Masukkan judul post"
                />
                <!-- validation -->
                <div class="mt-2 alert alert-danger" v-if="validation.title">
                  {{ validation.title[0] }}
                </div>
              </div>

              <div class="form-group">
                <label for="content" class="font-weight-bold">CONTENT</label>
                <textarea
                  rows="4"
                  class="form-control"
                  v-model="post.content"
                  placeholder="Masukkan konten post"
                ></textarea>
                <!-- validation -->
                <div class="mt-2 alert alert-danger" v-if="validation.content">
                  {{ validation.content[0] }}
                </div>
              </div>
              <button type="submit" class="btn btn-primary">SIMPAN</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, ref } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";

export default {
  setup() {
    // state posts
    const post = reactive({
      title: "",
      content: "",
    });

    // state validation
    const validation = ref([]);

    // vue router
    const router = useRouter();

    // route
    const route = useRoute();

    // mounted
    onMounted(() => {
      // get API from laravel backend
      axios
        .get(`https://simplelaravuecrud.herokuapp.com/api/post/${route.params.id}`)
        .then((response) => {
          // assign state post with response data
          post.title = response.data.data.title;
          post.content = response.data.data.content;
        })
        .catch((e) => {
          console.log(e.response.data);
        });
    });

    // method update
    function update() {
      let title = post.title;
      let content = post.content;

      axios
        .put(`https://simplelaravuecrud.herokuapp.com/api/post/${route.params.id}`, {
          title: title,
          content: content,
        })
        .then(() => {
          // redirect ke post index
          router.push({
            name: "post.index",
          });
        })
        .catch((e) => {
          // assign state validation with error
          validation.value = e.response.data;
        });
    }

    return {
      post,
      validation,
      router,
      update,
    };
  },
};
</script>

<style>
body {
  background: lightgray;
}
</style>
