<template>
  <div class="story">
    <div v-for="story in stories" :key="story.id">{{ story.title }}</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      stories: [],
    };
  },
  mounted: function () {
    this.indexStories();
  },
  methods: {
    indexStories: function () {
      axios.get("/stories").then((response) => {
        console.log(response.data);
        this.stories = response.data;
      });
    },
  },
};
</script>
