<template>
  <div class="character-selector">
    <div class="list-group">
      <a class="list-group-item character" v-on:click="showCharacter = !showCharacter">
        {{ character.name }}
        <div v-show="showCharacter">
          Age:
          <input type="text" v-model="character.age" />
          Description:
          <input type="text" v-model="character.description" />
        </div>
      </a>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: ["character"],
  data: function () {
    return {
      showCharacter: false,
      currentCharacter: {},
    };
  },
  methods: {
    updateCharacter: function (character) {
      let params = {
        name: character.name,
        age: character.age,
        description: character.description,
      };
      axios.patch("/characters" + character.id, params).then((response) => {
        console.log("Success", reponse.data);
      });
    },
  },
};
</script>
