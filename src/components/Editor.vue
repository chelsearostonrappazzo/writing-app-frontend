<template>
  <div class="container editor">
    <div class="row">
      <div class="col-md-4">
        <div class="btn-group" role="group">
          <button type="button" class="btn btn-primary btn-group-sm outline" v-on:click="chapterModal()">
            +Chapter
          </button>
          <button type="button" class="btn btn-primary btn-group-sm outline">+Character</button>
          <button type="button" class="btn btn-primary btn-group-sm outline">+ Party Member</button>
        </div>
        <dialog id="add-chapter-modal">
          <form method="dialog">
            <label>Title</label>
            <input type="text" v-model="title" />
            <button v-on:click="newChapter()" class="btn btn-primary outline">Add</button>
            <button class="btn btn-primary outline">Close</button>
          </form>
        </dialog>
        <div class="list-group">
          <a class="list-group-item" v-for="chapter in chapters" :key="chapter.id" v-on:click="selectChapter(chapter)">
            {{ chapter.title }}
          </a>
        </div>
      </div>
      <div class="col-md-8">
        {{ this.displayChapter.body }}
        <editor-content :editor="editor" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Editor, EditorContent } from "@tiptap/vue-2";
import StarterKit from "@tiptap/starter-kit";

export default {
  components: {
    EditorContent,
  },
  data: function () {
    return {
      selectedChapterId: "",
      chapters: [],
      displayChapter: "",
      editor: null,
      title: "",
    };
  },
  mounted: function () {
    this.indexChapter();
    this.setupTipTap();
  },
  methods: {
    setUpTipTap: function () {
      this.editor = new Editor({
        content: "<p>{{this.displayChapter}}</p>",
        extensions: [StarterKit],
      });
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
        this.displayChapter = this.chapter;
      });
    },
    chapterModal: function () {
      document.querySelector("#add-chapter-modal").showModal();
    },
    newChapter: function () {
      let params = {
        title: this.title,
        story_id: this.$route.params.id,
      };
      axios.post("/stories/" + this.$route.params.id + "/chapters", params).then((response) => {
        console.log(response.data);
        this.chapters.push(response.data);
      });
    },
  },
};
</script>
