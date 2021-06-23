<template>
  <div class="container editor">
    <div class="row">
      <div class="col-md-3">
        <div class="list-group">
          <a class="list-group-item" v-for="chapter in chapters" :key="chapter.id" v-on:click="selectChapter(chapter)">
            {{ chapter.title }}
          </a>
        </div>
      </div>
      <div class="col-md-6">
        <div id="scrolling-container">
          <div id="quill-container">
            <div id="editor-container">
              <p></p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* global Quill */
import axios from "axios";

export default {
  data: function () {
    return {
      selectedChapterId: "",
      chapters: [],
      displayChapter: "",
      quill: null,
    };
  },
  mounted: function () {
    this.setUpEditor();
    this.indexChapter();
  },
  methods: {
    setUpEditor: function () {
      var quill = new Quill("#quill-container", {
        modules: {
          toolbar: [[{ header: [1, 2, false] }], ["bold", "italic", "underline"], ["image", "code-block"]],
        },
        scrollingContainer: "#scrolling-container",
        theme: "snow",
      });

      console.log(quill);
    },
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
      });
    },
  },
};
</script>
