<template>
  <div class="list-group">
    <h2>Characters</h2>
    <a
      class="list-group-item"
      v-bind:class="{ active: selectedCharacterId === character.id }"
      v-on:click="selectCharacter(character)"
    >
      {{ character.title }}
      <span v-show="showCharacter">{{ character.age }}{{ character.description }}</span>
    </a>
  </div>
</template>

<script>
export default {
  props: ["selectedCharacterId", "character"],
  data: function () {
    return {
      showCharacter: false,
    };
  },
  methods: {
    selectCharacter: function (character) {
      this.$emit("selectCharacter", character);
    },
  },
};
</script>
