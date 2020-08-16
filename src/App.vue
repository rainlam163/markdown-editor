<template>
  <div id="app">
    <div class="markdown-editor">
      <div class="title">
        <div class="name">Markdown 在线编辑器</div>
        <div class="theme">
          主题：
          <select @change="onThemeChange($event)">
            <option value="vs-light">light</option>
            <option value="vs-dark" selected>dark</option>
          </select>
        </div>
      </div>
      <div id="monaco" class="monaco-editor"></div>
    </div>
    <div class="markdown-render">
      <div class="title">预览效果</div>
      <markdown-it-vue class="markdown-it" :content="content" />
    </div>
  </div>
</template>

<script>

import * as monaco from 'monaco-editor'
import MarkdownItVue from 'markdown-it-vue'
import 'markdown-it-vue/dist/markdown-it-vue.css'

export default {
  name: 'App',
  components: {
    MarkdownItVue
  },
  data() {
    return {
      editor: null,
      content: ''
    }
  },
  mounted() {
    monaco.editor.setTheme('vs-dark')
    this.editor = monaco.editor.create(document.getElementById('monaco'), {
      value: null,
      language: 'markdown',
      automaticLayout: true
    })
    this.editor.onDidChangeModelContent(() => {
      this.content = this.editor.getValue()
    })
  },
  methods: {
    onThemeChange(val) {
      monaco.editor.setTheme(val.target.value)
    }
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
#app {
  display: flex;
}
#app .title {
  display: flex;
  padding: 12px;
  justify-content: space-between;
  font-weight: bold;
}
#app .markdown-editor {
  width: 50%;
  overflow: hidden;
}
#app .markdown-editor .monaco-editor {
  height: calc(100vh - 46px);
  width: 100%;
}
#app .markdown-render {
  width: 50%;
}
#app .markdown-render .markdown-it {
  height: calc(100vh - 46px);
  overflow: auto;
  padding: 0 12px;
}
</style>