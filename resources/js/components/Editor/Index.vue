<template>
    <div class="md:flex w-full h-full justify-center items-center code-editor">
        <div class=" md:w-2/3  h-full">
            <div class="ace-container h-full">
                <div class="ace-editor relative h-full" ref="ace"></div>
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
    import {Mode as JavaScriptMode} from 'ace-builds/src-min-noconflict/mode-javascript'


    export default {
        props: {
            value: String
        },
        mounted() {
            this.aceEditor = ace.edit(this.$refs.ace, {
                value: this.value ? this.value : '',
                theme: 'ace/theme/twilight',
                tabSize: 4
            });

            this.aceEditor.setOptions({
                enableSnippets: true,
                enableLiveAutocompletion: true,
                enableBasicAutocompletion: true
            });

            // const marker = this.aceEditor.getSession().addDynamicMarker({
            //     id: "marker-layer",
            //     update: function(html, markerLayer, session, config) {
            //         return html;
            //     }
            // },true)

            // this.aceEditor.getSession().addMarker(new ace.Range(0, 0, 0, 0),'mark-layer', "fullLine", true)
            // console.log(this.aceEditor.getSession().addDynamicMarker);

            this.aceEditor.session.setMode(new JavaScriptMode())
            this.aceEditor.getSession().on('change', this.change)
        },
        data() {
            return {
                aceEditor: null,
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

// .marker-layer {
//         animation: blink 1s infinite;
//         background-color: red;
//         border: 1px solid red;
//     }

//     @keyframes blink {
//     0% {
//         opacity: 1;
//     }
//     50% {
//         opacity: 0;
//     }
//     100% {
//         opacity: 1;
//     }
//     }
  .code-editor{
    position: relative;
    height: 90vh;
  }
    .ace-container {
      position: relative;
      width: 100%;
    }
  </style>
