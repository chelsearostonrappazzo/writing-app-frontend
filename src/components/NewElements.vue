<template>
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-primary btn-group-sm outline" v-on:click="chapterModal()">
      <i class="ri-add-line"></i>
      Chapter
    </button>
    <button type="button" class="btn btn-primary btn-group-sm outline" v-on:click="characterModal()">
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
    <dialog id="add-character-modal">
      <form method="dialog">
        <ul class="list-group">
          <li class="list-group-item">
            <label>Title</label>
            <input type="text" v-model="name" />
          </li>
          <li class="list-group-item">
            <label>Age</label>
            <input type="text" v-model="age" />
          </li>
          <li class="list-group-item">
            <label>Description</label>
            <input type="text" v-model="description" />
          </li>
        </ul>
        <button v-on:click="newCharacter()" class="btn btn-primary outline">Add</button>
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
    return {
      name: "",
      age: "",
      description: "",
    };
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
    characterModal: function () {
      document.querySelector("#add-character-modal").showModal();
    },
    newCharacter: function () {
      let params = {
        name: this.name,
        age: this.age,
        description: this.description,
        story_id: this.$route.params.id,
      };
      axios.post("/stories/" + this.$route.params.id + "/characters", params).then((response) => {
        console.log(response.data);
        this.characters.push(response.data);
      });
    },
  },
};
</script>
