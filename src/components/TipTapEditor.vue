<template>
  <div class="tiptap-editor">
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleBold().run()"
      :class="{ 'is-active': editor.isActive('bold') }"
    >
      <i class="ri-bold"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleItalic().run()"
      :class="{ 'is-active': editor.isActive('italic') }"
    >
      <i class="ri-italic"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleStrike().run()"
      :class="{ 'is-active': editor.isActive('strike') }"
    >
      <i class="ri-strikethrough"></i>
    </button>
    <!-- <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleCode().run()"
      :class="{ 'is-active': editor.isActive('code') }"
    >
      code
    </button> -->
    <!-- <button class="btn btn-primary btn-group-sm outline" @click="editor.chain().focus().unsetAllMarks().run()">
      clear marks
    </button>
    <button class="btn btn-primary btn-group-sm outline" @click="editor.chain().focus().clearNodes().run()">
      clear nodes
    </button> -->
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().setParagraph().run()"
      :class="{ 'is-active': editor.isActive('paragraph') }"
    >
      <i class="ri-paragraph"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
      :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"
    >
      <i class="ri-h-1"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
      :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }"
    >
      <i class="ri-h-2"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
      :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }"
    >
      <i class="ri-h-3"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleHeading({ level: 4 }).run()"
      :class="{ 'is-active': editor.isActive('heading', { level: 4 }) }"
    >
      <i class="ri-h-4"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleHeading({ level: 5 }).run()"
      :class="{ 'is-active': editor.isActive('heading', { level: 5 }) }"
    >
      <i class="ri-h-5"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleHeading({ level: 6 }).run()"
      :class="{ 'is-active': editor.isActive('heading', { level: 6 }) }"
    >
      <i class="ri-h-6"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleBulletList().run()"
      :class="{ 'is-active': editor.isActive('bulletList') }"
    >
      <i class="ri-list-unordered"></i>
    </button>
    <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleOrderedList().run()"
      :class="{ 'is-active': editor.isActive('orderedList') }"
    >
      <i class="ri-list-ordered"></i>
    </button>
    <!-- <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleCodeBlock().run()"
      :class="{ 'is-active': editor.isActive('codeBlock') }"
    >
      code block
    </button> -->
    <!-- <button
      class="btn btn-primary btn-group-sm outline"
      @click="editor.chain().focus().toggleBlockquote().run()"
      :class="{ 'is-active': editor.isActive('blockquote') }"
    >
      blockquote
    </button> -->
    <!-- <button class="btn btn-primary btn-group-sm outline" @click="editor.chain().focus().setHorizontalRule().run()">
      horizontal rule
    </button>
    <button class="btn btn-primary btn-group-sm outline" @click="editor.chain().focus().setHardBreak().run()">
      hard break
    </button> -->
    <button class="btn btn-primary btn-group-sm outline" @click="editor.chain().focus().undo().run()">
      <i class="ri-arrow-go-back-line"></i>
    </button>
    <button class="btn btn-primary btn-group-sm outline" @click="editor.chain().focus().redo().run()">
      <i class="ri-arrow-go-forward-line"></i>
    </button>

    <editor-content :editor="editor" />
    word count: {{ wordCount }}
  </div>
</template>

<script>
import { Editor, EditorContent } from "@tiptap/vue-2";
import StarterKit from "@tiptap/starter-kit";
import FontFamily from "@tiptap/extension-font-family";

export default {
  components: {
    EditorContent,
  },
  props: ["value"],
  data() {
    return {
      editor: null,
      wordCount: 0,
    };
  },
  watch: {
    value(value) {
      // const isSame = this.editor.getHTML() === value;
      // JSON
      const isSame = this.editor.getJSON().toString() === value.toString();
      if (isSame) {
        return;
      }

      this.editor.commands.setContent(value, true);
    },
  },

  mounted() {
    this.editor = new Editor({
      extensions: [StarterKit, FontFamily],
      content: this.value,
      editorProps: {
        attributes: {
          spellcheck: "true",
        },
      },

      onUpdate: () => {
        this.wordCount = this.editor.state.doc.textContent.match(/\b(\w+)\b/g).length;
        // this.$emit("input", this.editor.getHTML());
        // JSON

        this.$emit("input", this.editor.getJSON());
        console.log(this.wordCount);
      },
    });
  },

  beforeDestroy() {
    this.editor.destroy();
  },
};
</script>
