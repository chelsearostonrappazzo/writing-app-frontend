<template>
  <div class="list-group">
    <a
      class="list-group-item"
      v-bind:class="{ active: selectedChapterId === chapter.id }"
      v-on:click="selectChapter(chapter)"
    >
      {{ chapter.title }}
    </a>
  </div>
</template>

<script>
export default {
  props: ["selectedChapterId", "chapter"],
  data: function () {
    return {};
  },
  methods: {
    selectChapter: function (chapter) {
      this.$emit("selectChapter", chapter);
    },
  },
};
</script>
