<template>
  <div class="editor">
    <editor-floating-menu
      v-slot="{ commands, isActive, menu }"
      :editor="editor"
    >
      <div
        class="editor__floating-menu"
        :class="{
          'is-active': menu.isActive
        }"
        :style="`top: ${menu.top}px`"
      >
        <div class="buttons has-addons">
          <button
            class="menubar__button button is-small is-outlined"
            :class="{ 'is-active': isActive.heading({ level: 1 }) }"
            @click="commands.heading({ level: 1 })"
          >
            <strong>H1</strong>
          </button>
          <button
            class="menubar__button button is-small is-outlined"
            :class="{ 'is-active': isActive.heading({ level: 2 }) }"
            @click="commands.heading({ level: 2 })"
          >
            <strong>H2</strong>
          </button>
          <button
            class="menubar__button button is-small is-outlined"
            :class="{ 'is-active': isActive.heading({ level: 3 }) }"
            @click="commands.heading({ level: 3 })"
          >
            <strong>H3</strong>
          </button>
          <button
            class="menubar__button button is-small is-outlined"
            :class="{ 'is-active': isActive.bold() }"
            @click="commands.bold"
          >
            <strong>bold</strong>
          </button>
        </div>
      </div>
    </editor-floating-menu>
    <editor-content :editor="editor" />
    <div v-html="content"></div>
  </div>
</template>

<script>
import { Editor, EditorContent, EditorFloatingMenu } from 'tiptap'
import {
  Heading,
  Bold
} from 'tiptap-extensions'

export default {
  components: {
    EditorFloatingMenu,
    EditorContent
  },
  data() {
    return {
      editor: null,
      linkMenuIsActive: false,
      linkUrl: null,
      content: ''
    }
  },
  created() {
    this.editor = new Editor({
      content: 'Initial Content',
      extensions: [
        new Heading({ levels: [1, 2, 3] }),
        new Bold(),
      ],
      onUpdate: ({ getHTML }) => {
        this.content = getHTML()
      }
    })
  },
  beforeDestroy() {
    this.editor.destroy()
  }
}
</script>
<style scoped>
.editor {
  position: relative;
}

.editor__floating-menu {
  position: absolute;
  z-index: 1;
  margin-top: -1.6rem;
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.2s, visibility 0.2s;
}
.editor /deep/ .ProseMirror {
  text-align: left;
}

.editor__floating-menu.is-active {
  opacity: 1;
  visibility: visible;
}
</style>
