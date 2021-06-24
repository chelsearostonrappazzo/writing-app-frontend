<template>
  <div class="container editor">
    <div class="row">
      <div class="col-md-3">
        <NewElements />
        <ChapterSelector />
      </div>
      <div class="col-md-6">
        <!-- <div v-if="this.displayChapter"> -->
        <TipTapEditor v-bind:displayChapter="value" v-model="displayChapter" />
        <button v-on:click="updateChapter(chapter, displayChapter)">save</button>
        <!-- </div> -->
        <!-- <div v-else>
          <TipTapEditor />
          <button v-on:submit.prevent="updateChapter(chapter, displayChapter)">save</button>
        </div> -->
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
  props: ["value", "title", "chapters", "selectedChapterId", "chapter"],
  data: function () {
    return {
      displayChapter: {},
    };
  },
  methods: {
    updateChapter: function (chapter, displayChapter) {
      let params = {
        title: chapter.title,
        body: displayChapter,
      };
      axios.patch("/chapters/" + chapter.id, params).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>
