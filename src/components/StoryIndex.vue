<template>
  <div class="container story">
    <div class="row">
      <div class="card story" v-for="story in stories" :key="story.id">
        <div class="card-body">
          <h5 class="card-title">{{ story.title }}</h5>
          <p class="card-text">
            {{ story.description }}
          </p>
          <router-link :to="`/stories/${story.id}/chapters`" class="card-link">Chapters</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      stories: [],
      selectedStoryId: 1,
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
    selectStory: function (story) {
      this.selectedStoryId = story.id;
    },
  },
};
</script>
