<template>
  <div class="home">
    <h1>Let's Get Litty</h1>
    <div id="standalone-container">
      <div id="toolbar-container">
        <span class="ql-formats">
          <select class="ql-font">
            <option selected>Sans Serif</option>
            <option value="georgia">Georgia</option>
            <option value="garamond">Garamond</option>
          </select>
          <select class="ql-size"></select>
        </span>
        <span class="ql-formats">
          <button class="ql-bold"></button>
          <button class="ql-italic"></button>
          <button class="ql-underline"></button>
          <button class="ql-strike"></button>
        </span>
        <span class="ql-formats">
          <select class="ql-color"></select>
          <select class="ql-background"></select>
        </span>
        <span class="ql-formats">
          <button class="ql-blockquote"></button>
          <button class="ql-code-block"></button>
          <button class="ql-link"></button>
        </span>
        <span class="ql-formats">
          <button class="ql-header" value="1"></button>
          <button class="ql-header" value="2"></button>
        </span>
        <span class="ql-formats">
          <button class="ql-list" value="ordered"></button>
          <button class="ql-list" value="bullet"></button>
          <button class="ql-indent" value="-1"></button>
          <button class="ql-indent" value="+1"></button>
        </span>
        <span class="ql-formats">
          <button class="ql-direction" value="rtl"></button>
          <select class="ql-align"></select>
        </span>
        <span class="ql-formats">
          <button class="ql-script" value="sub"></button>
          <button class="ql-script" value="super"></button>
        </span>
        <span class="ql-formats">
          <button class="ql-clean"></button>
        </span>
      </div>
    </div>
    <div id="scrolling-container">
      <div id="editor-container">
        <p>
          <br />
          <ChapterShow />
        </p>
      </div>
    </div>
  </div>
</template>
<style>
.home {
  margin: 0 auto;
  height: 500px;
  width: 75%;
}
#toolbar-container .ql-font span[data-label="Georgia"]::before {
  font-family: "Georgia";
}
#toolbar-container .ql-font span[data-label="Garamond"]::before {
  font-family: "Garamond";
}
.ql-font-georgia {
  font-family: "Georgia";
}
.ql-font-garamond {
  font-family: "Garamond";
}
#scrolling-container {
  height: 100%;
  min-height: 100%;
  overflow-y: auto;
}
</style>
<script>
/* global Quill */
import ChapterShow from "@/components/ChapterShow";
export default {
  components: {
    ChapterShow,
  },
  mounted: function () {
    this.setUpEditor();
  },
  methods: {
    setUpEditor: function () {
      var Font = Quill.import("formats/font");
      Font.whitelist = ["georgia", "garamond"];
      Quill.register(Font, true);
      var Delta = Quill.import("delta");
      var quill = new Quill("#editor-container", {
        modules: {
          toolbar: "#toolbar-container",
          history: {
            delay: 2000,
            maxStack: 500,
            userOnly: true,
          },
        },
        scrollingContainer: "#scrolling-container",
        placeholder: "Write your heart out...",
        theme: "snow",
      });
      console.log(quill);
      var change = new Delta();
      quill.on("text-change", function (delta) {
        change = change.compose(delta);
      });
      setInterval(function () {
        if (change.length() > 0) {
          console.log("Saving changes", change);
          change = new Delta();
        }
      }, 5 * 1000);
      window.onbeforeunload = function () {
        if (change.length() > 0) {
          return "There are unsaved changes. Are you sure you want to leave?";
        }
      };
    },
  },
};
</script>
