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

        <CharacterSelector
          v-bind:character="character"
          v-for="character in characters"
          :key="character.id"
          v-bind:selectedCharacterId="selectedCharacterId"
          v-on:selectCharacter="selectCharacter"
        />
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
      selectedCharacterId: "",
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
    selectCharacter: function (character) {
      this.selectedCharacterId = character.id;
      axios.get("/characters/" + this.selectedCharacterId).then((response) => {
        console.log(response.data);
        character = response.data;
        this.displayCharacter = character.body;
        console.log(this.displayCharacter);
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
