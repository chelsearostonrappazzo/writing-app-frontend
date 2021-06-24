<template>
  <div class="container editor">
    <div class="row">
      <div class="col-md-3">
        <div class="btn-group" role="group">
          <button type="button" class="btn btn-primary btn-group-sm outline" v-on:click="chapterModal()">
            <i class="ri-add-line"></i>
            Chapter
          </button>
          <button type="button" class="btn btn-primary btn-group-sm outline">
            <i class="ri-user-add-line"></i>
            Character
          </button>
          <button type="button" class="btn btn-primary btn-group-sm outline">
            <i class="ri-user-add-line"></i>
            Party Member
          </button>
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
      <div class="col-md-6">
        <div v-if="!chapter.body">
          <TipTapEditor />
          <button v-on:click="updateChapter(chapter)">save</button>
        </div>
        <div v-else>
          <TipTapEditor v-model="chapter.body" />
          <button v-on:submit.prevent="updateChapter(chapter)">save</button>
        </div>
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

export default {
  components: {
    TipTapEditor,
    WritersBlock,
  },
  data: function () {
    return {
      selectedChapterId: "",
      chapters: [],
      displayChapter: "",
      title: "",
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
      axios.get("/chapters/" + chapter.id).then((response) => {
        console.log(response.data);
        this.chapter = response.data;
        this.displayChapter = this.chapter.body;
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
    updateChapter: function (chapter) {
      let params = {
        title: chapter.title,
        body: chapter.body,
        id: chapter.id,
      };
      axios.patch("/chapters/" + chapter.id, params).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>
