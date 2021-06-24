<template>
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
    <dialog id="add-chapter-modal">
      <form method="dialog">
        <label>Title</label>
        <input type="text" v-model="title" />
        <button v-on:click="newChapter()" class="btn btn-primary outline">Add</button>
        <button class="btn btn-primary outline">Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["title"],
  data: function () {
    return {};
  },
  methods: {
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
