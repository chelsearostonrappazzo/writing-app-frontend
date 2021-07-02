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
        <h2>Characters</h2>
        <CharacterSelector v-bind:character="character" v-for="(character, index) in characters" :key="index + 10" />
      </div>
      <div class="col-md-6">
        <div v-if="displayChapter">
          <TipTapEditor v-bind:displayChapter="displayChapter" v-model="displayChapter" />
        </div>
        <div v-else>
          <TipTapEditor v-model="displayChapter" />
        </div>
        <button v-on:click="updateChapter(chapter)">save</button>
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
import CharacterSelector from "@/components/CharacterSelector";

export default {
  components: {
    TipTapEditor,
    WritersBlock,
    NewElements,
    ChapterSelector,
    CharacterSelector,
  },

  data() {
    return {
      selectedChapterId: "",
      displayChapter: "",
      chapters: [],
      title: "",
      chapter: {},
      character: {},
      characters: [],
    };
  },

  mounted() {
    this.indexChapter();
    this.indexCharacter();
  },

  methods: {
    indexChapter: function () {
      axios.get("/stories/" + this.$route.params.id + "/chapters").then((response) => {
        console.log(response.data);
        this.chapters = response.data;
      });
    },
    indexCharacter: function () {
      axios.get("/stories/" + this.$route.params.id + "/characters").then((response) => {
        console.log(response.data);
        this.characters = response.data;
      });
    },
    selectChapter: function (chapter) {
      this.selectedChapterId = chapter.id;
      axios.get("/chapters/" + this.selectedChapterId).then((response) => {
        console.log(response.data);
        chapter = response.data;
        this.displayChapter = chapter.body;
        console.log(this.displayChapter);
      });
    },
    updateChapter: function (chapter) {
      let params = {
        title: chapter.title,
        body: this.displayChapter,
      };
      axios.patch("/chapters/" + this.selectedChapterId, params).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>
