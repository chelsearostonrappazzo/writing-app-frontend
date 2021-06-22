<template>
  <div class="container dashboard">
    <div class="row">
      <div class="col-md-3">
        <ChapterIndex />
      </div>
      <div class="col-md-8">
        <Editor />
      </div>
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
