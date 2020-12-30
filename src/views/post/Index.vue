<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>DATA POST</h4>
            <hr />
            <router-link :to="{ name: 'post.create' }" class="btn btn-md btn-success"
              >TAMBAH POST</router-link
            >

            <table class="table table-striped table-bordered mt-4">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">TITLE</th>
                  <th scope="col">CONTENT</th>
                  <th scope="col">OPTION</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(post, index) in posts" :key="index">
                  <td>{{ post.title }}</td>
                  <td>{{ post.content }}</td>
                  <td class="text-center">
                    <router-link
                      class="btn btn-sm btn-primary mr-1"
                      :to="{ name: 'post.edit', params: { id: post.id } }"
                      >EDIT
                    </router-link>
                    <button
                      @click.prevent="postDelete(post.id)"
                      class="btn btn-sm btn-danger ml-1"
                    >
                      DELETE
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { onMounted, ref } from "vue";

export default {
  setup() {
    // reactive state
    let posts = ref([]);

    // mounted
    onMounted(() => {
      // get API from laravel back end
      axios
        .get("https://simplelaravuecrud.herokuapp.com/api/post")

        .then((response) => {
          // asign state posts with response data
          posts.value = response.data.data;
        })
        .catch((e) => {
          console.log(e.response.data);
        });
    });

    // delete function
    function postDelete(id) {
      axios
        .delete(`https://simplelaravuecrud.herokuapp.com/api/post/${id}`)
        .then(() => {
          // splice post
          posts.value.splice(posts.value.indexOf(id), 1);
        })
        .catch((e) => {
          console.log(e.response.data);
        });
    }

    // return
    return {
      posts,
      postDelete,
    };
  },
};
</script>

<style>
body {
  background: lightgray;
}
</style>
