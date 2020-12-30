<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>Tambah Post</h4>
            <hr />

            <form @submit.prevent="store">
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

                <div class="form-group mt-3">
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
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";

export default {
  setup() {
    // state post
    const post = reactive({
      title: "",
      content: "",
    });

    // state validation
    const validation = ref([]);

    // vue router
    const router = useRouter();

    // method store
    function store() {
      let title = post.title;
      let content = post.content;

      axios
        .post("https://simplelaravuecrud.herokuapp.com/api/post", {
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
      store,
    };
  },
};
</script>

<style>
body {
  background: lightgray;
}
</style>
