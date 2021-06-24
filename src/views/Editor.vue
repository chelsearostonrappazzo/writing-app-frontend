<template>
  <div class="container editor">
    <div class="row">
      <div class="col-md-3">
        <NewElements />
        <ChapterSelector
          v-bind:chapter="chapter"
          v-for="chapter in chapters"
          :key="chapter.id"
          v-bind:selectedChapterId="selectedChapterId"
          v-on:selectChapter="selectChapter"
        />
      </div>
      <div class="col-md-6">
        <TipTapEditor v-bind:displayBody="modelValue" v-model="displayBody" />
        <button v-on:click="updateChapter(chapter, displayBody)">save</button>
      </div>
      <div class="col-md-3">
        <WritersBlock />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import TipTapEditor from "@/components/TipTapEditor";
import WritersBlock from "@/components/WritersBlock";
import NewElements from "@/components/NewElements";
import ChapterSelector from "@/components/ChapterSelector";

export default {
  components: {
    TipTapEditor,
    WritersBlock,
    NewElements,
    ChapterSelector,
  },

  data: function () {
    return {
      displayBody: {},
      selectedChapterId: 1,
      modelValue: "",
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
    updateChapter: function (chapter, displayBody) {
      let params = {
        title: chapter.title,
        body: displayBody,
      };
      axios.patch("/chapters/" + chapter.id, params).then((response) => {
        console.log(response.data);
      });
    },
    selectChapter: function (chapter) {
      this.selectedChapterId = chapter.id;
      axios.get("/chapters/" + this.selectedChapterId).then((response) => {
        console.log(response.data);
        chapter = response.data;
        this.displayBody = chapter.body;
        console.log(this.displayBody);
      });
    },
  },
};
</script>
