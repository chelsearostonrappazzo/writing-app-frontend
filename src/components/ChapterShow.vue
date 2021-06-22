<template>
  <div class="chapter">
    <p>{{ chapter.body }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      chapter: {},
      selectedChapterId: 1,
    };
  },
  mounted: function () {
    this.showChapter();
  },
  methods: {
    showChapter: function () {
      axios.get("/chapters/" + this.selectedChapterId).then((response) => {
        console.log(response.data);
        this.chapter = response.data;
      });
    },
  },
};
</script>
