<template>
    <div class="md:flex w-full h-full justify-center items-center ">
        <div class=" md:w-2/3 code-editor">
            <div class="ace-container">
                <div class="ace-editor" ref="ace"></div>
            </div>
        </div>
    </div>
  </template>

  <script>
      import ace from 'ace-builds'
      import 'ace-builds/src-noconflict/snippets/javascript'
      import 'ace-builds/src-noconflict/snippets/html'
      import 'ace-builds/src-noconflict/snippets/css'
      import 'ace-builds/src-noconflict/snippets/scss'
      import 'ace-builds/src-noconflict/snippets/json'
      import 'ace-builds/src-noconflict/snippets/java'
      import 'ace-builds/src-noconflict/snippets/text'
      import 'ace-builds/webpack-resolver'
      import 'ace-builds/src-noconflict/ext-language_tools'
      import 'ace-builds/src-noconflict/theme-monokai'
      import 'ace-builds/src-noconflict/mode-javascript'

      export default {
          props: {
              value: String
          },
          mounted() {
              this.aceEditor = ace.edit(this.$refs.ace, {
                  maxLines: 1300,
                  minLines: 50,
                  fontSize: 14,
                  value: this.value ? this.value : '',
                  mode:'ace/mode/javascript',
                  theme: 'ace/theme/twilight',
                  tabSize: 4
              });

              this.aceEditor.setOptions({
                  enableSnippets: true,
                  enableLiveAutocompletion: true,
                  enableBasicAutocompletion: true
              });

              this.aceEditor.getSession().on('change', this.change)
          },
          data() {
              return {
                  aceEditor: null,
                  themePath: 'ace/theme/monokai',
                  modePath: 'ace/mode/javascript',
              }
          },
          methods: {
              change() {
                  this.$emit('input', this.aceEditor.getSession().getValue())
              }
          }
      }
  </script>

  <style lang='scss' scoped>

  .code-editor{

  }
    .ace-container {
      position: relative;
      width: 100%;
    }
  </style>
