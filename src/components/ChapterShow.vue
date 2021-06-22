<template>
  <div class="story">
    <div>{{ chapter.body }}</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      chapter: {},
    };
  },
  mounted: function () {
    this.showChapter();
  },
  methods: {
    showChapter: function () {
      axios.get("/chapters/" + this.$route.params.id).then((response) => {
        console.log(response.data);
        this.chapter = response.data;
      });
    },
  },
};
</script>
