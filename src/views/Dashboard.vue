<template>
  <div class="container dashboard">
    <div class="row">
      <ChapterIndex />
      <Editor />
    </div>
  </div>
</template>

<script>
import ChapterIndex from "@/components/ChapterIndex";
import Editor from "@/components/Editor";
export default {
  components: {
    ChapterIndex,
    Editor,
  },
};
</script>
