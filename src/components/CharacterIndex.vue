<template>
  <div class="chapter-index">
    <div class="row">
      <div class="list-group">
        <h2>Characters</h2>
        <a
          class="list-group-item"
          v-for="character in characters"
          :key="character.id"
          v-on:click="selectCharacter(character)"
        >
          {{ character.name }}
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      characters: [],
    };
  },
  mounted: function () {
    this.indexCharacter();
  },
  methods: {
    indexCharacter: function () {
      axios.get("/stories/" + this.$route.params.id + "/characters").then((response) => {
        console.log(response.data);
        this.characters = response.data;
      });
    },
  },
};
</script>
