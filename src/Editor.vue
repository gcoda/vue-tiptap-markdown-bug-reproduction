<template>
  <div class="min-vh-100 p-4">
    <div class="row h-100">
      <button @click="setContent">
        this.editor.commands.setContent
        <!--  -->
        (`## This should be markdown\n\n Aint no mark`)
      </button>
      <div class="col-md-6">
        <textarea
          class="form-control h-100"
          rows="10"
          ref="markdown"
          v-model="markdown"
          @input="handleInput"
        ></textarea>
      </div>
      <div class="col-md-6">
        <div class="editor card h-100">
          <div class="card-header sticky-top bg-white"></div>

          <EditorContent
            class="editor__content flex-fill"
            style="margin: -1px"
            :editor="editor"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
import Table from '@tiptap/extension-table'
import TableRow from '@tiptap/extension-table-row'
import TableCell from '@tiptap/extension-table-cell'
import TableHeader from '@tiptap/extension-table-header'
import Underline from '@tiptap/extension-underline'
import Image from '@tiptap/extension-image'
import Link from '@tiptap/extension-link'
import Highlight from '@tiptap/extension-highlight'
import TaskList from '@tiptap/extension-task-list'
import TaskItem from '@tiptap/extension-task-item'
import CodeBlock from '@tiptap/extension-code-block'
import { createMarkdownEditor } from 'tiptap-markdown'
import { content } from './content'
export default {
  components: {
    EditorContent,
  },
  data() {
    return {
      editor: null,
      markdown: null,
    }
  },
  methods: {
    updateMarkdownOutput() {
      this.markdown = this.editor.getMarkdown()
    },
    handleInput() {
      this.editor.commands.setContent(this.markdown)
    },
    setContent() {
      this.editor.commands.setContent(
        `## This should be markdown\n\n Aint no mark`
      )
    },
  },
  mounted() {
    const MarkdownEditor = createMarkdownEditor(Editor)
    this.editor = new MarkdownEditor({
      extensions: [
        StarterKit.configure({
          codeBlock: false,
        }),
        Table.configure({
          resizable: false,
        }),
        TableRow,
        TableHeader,
        TableCell,
        Underline,
        Image.configure({
          inline: true,
        }),
        Link,
        Highlight,
        TaskList,
        TaskItem,
        CodeBlock.configure({
          languageClassPrefix: 'lang-',
        }),
      ],
      content,
      onUpdate: () => {
        this.updateMarkdownOutput()
      },
    })
    this.editor.view.dom.classList.add('card-body', 'form-control', 'h-100')
    this.updateMarkdownOutput()
  },
}
</script>
