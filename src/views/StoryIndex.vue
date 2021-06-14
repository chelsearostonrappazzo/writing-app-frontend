<template>
  <div class="story">
    <div v-for="story in stories" :key="story.id">{{story.name}}
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      stories: [],
      
    };
  },
  methods: {
    indexStories: function () {
      axios.get("/api/stories").then((response) => {console.log(response.data); 
      this.stories = response.data;
      });
    }
  }
}
<script>
