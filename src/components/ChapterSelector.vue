<template>
  <div class="list-group">
    <a class="list-group-item" v-for="chapter in chapters" :key="chapter.id" v-on:click="selectChapter(chapter)">
      {{ chapter.title }}
    </a>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      chapters: [],
      selectedChapterId: "",
      chapter: {},
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
    selectChapter: function (chapter) {
      this.selectedChapterId = chapter.id;
      axios.get("/chapters/" + this.selectedChapterId).then((response) => {
        console.log(response.data);
        this.chapter = response.data;
        this.displayChapter = this.chapter.body;
        console.log(this.displayChapter);
      });
    },
  },
};
</script>
