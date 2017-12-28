<template>
  <div>
    <codemirror ref="myEditor" v-model="code"></codemirror>
    <button @click="insertAfter('')">文頭削除</button>
    <button @click="insertAfter('です。')">です。</button>
    <button @click="insertAfter('してしまいます。')">してしまいます。</button>
    <button @click="insertAfter('でしょう。')">でしょう。</button>
    <button @click="insertAfter('ということです。')">ということです。</button>
    <button @click="insertAfter('と考えられます。')">と考えられます。</button>
    <button @click="insertAfter('ということも事実です。')">ということも事実です。</button>
    <button @click="insertAfter('ということがよくあります。')">ということがよくあります。</button>
    <button @click="insertAfter('について')">について</button>
  </div>
</template>
<style>
  body{
    background: #EEE;
  }
</style>
<script>
  import { codemirror } from 'vue-codemirror-lite'

  export default {
    components: {
      codemirror
    },
    data() {
      return {
        code: '- テスト\n- なのかなあ\n  - まあいいか\n'
      }
    },
    computed: {
      editor() {
        // get current editor object
        return this.$refs.myEditor.editor
      }
    },
    methods: {
      removeListHeader(){
        const cursor = this.editor.getCursor();
        const line = this.editor.getLine(cursor.line);
        const from = {
          line: cursor.line,
          ch: 0
        }
        const to = {
          line: cursor.line,
          ch: line.length
        }
        const selected = this.editor.getRange(from, to);
        const replaced = selected.replace(/^[ -]*/, "")
        this.editor.replaceRange(replaced, from, to); // adds a new line
      },
      insertAfter(text){
        this.removeListHeader()
        const cursor = this.editor.getCursor();
        const line = this.editor.getLine(cursor.line);
        const from = {
          line: cursor.line,
          ch: line.length
        }
        const to = {
          line: cursor.line,
          ch: line.length
        }
        this.editor.replaceRange(text, from, to); // adds a new line
        this.editor.setSelection({
          line: cursor.line + 1,
          ch: 0
        })
        this.editor.focus()
      },
      insertBefore() {
      }
    },
    mounted() {
      // use editor object...
      this.editor.focus()
      console.log('this is current editor object', this.editor)
    }
  }
</script>