<template>
  <div class="chapter-index">
    <div class="row">
      <ul class="list-group">
        <li class="list-group-item" v-for="chapter in chapters" :key="chapter.id">{{ chapter.title }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      chapters: [],
    };
  },
  mounted: function () {
    this.indexChapter();
  },
  methods: {
    indexChapter: function () {
      axios.get("/stories/" + this.$route.params.id + "/chapters").then((response) => {
        console.log(response.data);
        this.chapters = response.data;
      });
    },
  },
};
</script>
