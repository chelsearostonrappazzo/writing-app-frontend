<template>
  <div class="chapter">
    <p>{{ chapter.body }}</p>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      chapter: {},
      selectedChapterId: "",
    };
  },
  mounted: function () {},
  methods: {},
};
</script>
