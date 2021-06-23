<template>
  <div class="chapter-selector">
    <ul class="list-group">
      <li
        class="list-group-item"
        v-bind:class="{ active: selectedChapterId === chapter.id }"
        v-on:click="selectChapter(chapter)"
      >
        {{ chapter.title }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["chapter", "selectedChapterId"],
  methods: {
    selectChapter: function (chapter) {
      this.$emit("selectChapter", chapter);
    },
  },
};
</script>
