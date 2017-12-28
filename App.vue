<template>
  <div>
    <h1>Hello!</h1>
    <button @click="insertBefore('です')">です。</button>
    <codemirror ref="myEditor" v-model="code"></codemirror>
  </div>
</template>
<script>
  import { codemirror } from 'vue-codemirror-lite'

  export default {
    components: {
      codemirror
    },
    data() {
      return {
        code: 'const str = "hello world"'
      }
    },
    computed: {
      editor() {
        // get current editor object
        return this.$refs.myEditor.editor
      }
    },
    methods: {
      insertBefore() {
        //this.editor.replaceRange("foo", {ch: Infinity});  
        var doc = this.editor
        var cursor = doc.getCursor(); // gets the line number in the cursor position
        var line = doc.getLine(cursor.line); // get the line contents
        var pos = {
          line: cursor.line,
          ch: line.length
        }
        var from = {
          line: cursor.line,
          ch: 0
        }
        var to = {
          line: cursor.line,
          ch: line.length
        }
        doc.replaceRange('' + "data" + '', from, to); // adds a new line
        doc.setSelection({
          line: cursor.line + 1,
          ch: 0
        })
        this.editor.focus()
        
        /*
        this.editor.replaceRange("foo\n", {line: Infinity});      
        
        */
        //console.log(this.editor.getSelection())
        /*
        CodeMirror.Pos(editor.getCursor().line,editor.getCursor().ch)
        console.log(this.editor.getCursor().line)
        */
      }
    },
    mounted() {
      // use editor object...
      this.editor.focus()
      console.log('this is current editor object', this.editor)
    }
  }
</script>