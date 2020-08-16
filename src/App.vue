<template>
  <div id="app">
    <div id="monaco" class="monaco-editor"></div>
    <markdown-it-vue class="markdown-render" :content="content" :options="options" />
  </div>
</template>

<script>

import * as monaco from 'monaco-editor'
import MarkdownItVue from 'markdown-it-vue'
export default {
  name: 'App',
  components: {
    MarkdownItVue
  },
  data() {
    return {
      editor: null,
      options: {
        markdownIt: {
          linkify: true
        },
        linkAttributes: {
          attrs: {
            target: '_blank',
            rel: 'noopener'
          }
        }
      },
      content: ''
    }
  },
  mounted() {
    this.editor = monaco.editor.create(document.getElementById('monaco'), {
      value: null
    })
    this.editor.onDidChangeModelContent(() => {
      this.content = this.editor.getValue()
    })
  },
  beforeDestroy() {
    this.editor.dispose()
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
.monaco-editor {
  width: 50%;
  height: 100vh;
  float: left;
  overflow: hidden;
}
.markdown-render {
  width: 50%;
  float: left;
  height: 100vh;
  overflow: auto;
}
</style>