<template>
  <div id="app">
    <div class="container">
      <div class="button-wrapper">
        <button class="btn" @click="searchUnsplash('Autumn')">Autumn</button>
        <button class="btn" @click="searchUnsplash('Motocycle')">Motocycle</button>
      </div>
      <stack :column-min-width="300" :gutter-width="15" :gutter-height="15" monitor-images-loaded>
        <stack-item v-for="(image, i) in images" :key="i" style="transition: transform 300ms">
          <img :src="image.urls.small" :alt="image.alt_description" />
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";

export default {
  name: "app",
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    images: []
  }),
  methods: {
    searchUnsplash(topic) {
      this.images = [];
      Axios.get(
        `https://api.unsplash.com/search/photos?query=${topic}&per_page=20`,
        {
          headers: {
            Authorization: `Client-ID ${process.env.VUE_APP_UNSPLASH}`,
            "Accept-Version": "v1"
          }
        }
      )
        .then(response => {
          this.images = response.data.results;
        })
        .catch(() => {
          this.images = [];
        });
    }
  }
};
</script>

<style>
.container {
  width: 80vw;
  margin: 0 auto;
}
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.bt {
  font-size: 18px;
  background-color: #42b983;
  color: white;
  padding: 10px 20px;
}

.btn:not(:last-child) {
  margin-right: 10px;
}

img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
</style>
