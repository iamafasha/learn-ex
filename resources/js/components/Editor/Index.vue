<template>
    <div class="md:flex w-full h-full justify-center items-center code-editor">
        <div class=" md:w-2/3  h-full">
            <div class="ace-container h-full">
                <div class="ace-editor relative h-full" ref="ace_text_area"></div>
            </div>
        </div>
    </div>
  </template>

  <script setup>
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
    import { reactive, ref, onMounted } from 'vue'


    const ace_text_area = ref(null)

    const props = defineProps({ value: String })

    const state = reactive({
        aceEditor:null,
        value:""
    })

    const change = () => {
                $emit('input', state.aceEditor.getSession().getValue())
    }

    onMounted(()=>{
        state.value = props.value
        state.aceEditor = ace.edit(ace_text_area.value, {
                value: state.value ? state.value : '',
                theme: 'ace/theme/twilight',
                tabSize: 4
            });

    var label = "I am red remote cursor :)";
    var marker_element = document.createElement("div");
    var cursor_element = document.createElement("div");
    cursor_element.className = "ace-multi-cursor";
    cursor_element.style.background = "red";
    marker_element.append(cursor_element);

    var tooltip_element = document.createElement("div");
    tooltip_element.className = "ace-multi-cursor-tooltip";
    tooltip_element.style.background = "red";
    tooltip_element.style.opacity = "1";
    tooltip_element.innerHTML = label;
    marker_element.append(tooltip_element);

    state.aceEditor.session.addDynamicMarker({
        contentNode: null,
        update: function(_, layer, session, config) {


            var top = layer.$getTop(5, config);
            var left = layer.$padding + 6 * config.characterWidth;
            var height = config.lineHeight;

            var cursor_top = top + 2;
            var cursor_height = height - 3;
            var cursor_left = left;
            var cursor_width = 2;

            // console.log(cursor_element)
            // cursor_element.style.position = "relative";
            cursor_element.style.height = `${cursor_height}px`;
            cursor_element.style.width = `${cursor_width}px`;
            cursor_element.style.top = `${cursor_top}px`;
            cursor_element.style.left = `${cursor_left}px`;

            let toolTipTop = cursor_top - height;
            if (toolTipTop < 5) {
            toolTipTop = cursor_top + height - 1;
            }

            const toolTipLeft = cursor_left;
            tooltip_element.style.top = `${toolTipTop - 2}px`;
            tooltip_element.style.left = `${toolTipLeft - 2}px`;

            // marker_element.remove();
            layer.elt("remote-cursor", "");
            const parentNode = layer.element.childNodes[layer.i - 1] || layer.element.lastChild;
            parentNode.appendChild(marker_element);
        }
    },true)

    state.aceEditor.session.setMode(new JavaScriptMode())
    state.aceEditor.getSession().on('change',() => change )
    })

  </script>

  <style lang='scss' scoped>
 .ace-editor {
	/*height: 150px;*/
	position: absolute;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
}

.other_user_selection {
	position: absolute;
	z-index: 6;
}

.other_user_selection.blue {
	background-color: blue;
	background-color: rgba(0, 0, 255, 0.5);
}

/*ace Collab ext*/
.ace-multi-cursor {
  position: absolute;
  pointer-events: auto;
  z-index: 10;
}

.ace-multi-cursor-tooltip {
  position: absolute;
  white-space: nowrap;
  color: #FFFFFF;
  text-shadow: 0 0 1px #000000;
  opacity: 1;
  font-size: 12px;
  padding: 2px;
  font-family: sans-serif;

  transition: opacity 0.5s ease-out;
  -webkit-transition: opacity 0.5s ease-out;
  -moz-transition: opacity 0.5s ease-out;
  -ms-transition: opacity 0.5s ease-out;
  -o-transition: opacity 0.5s ease-out;
}

.ace-multi-selection {
  position: absolute;
  pointer-events: auto;
  z-index: 10;
  opacity: 0.3;
}

.ace-radar-view {
  position: relative;
  min-width: 6px;
}

.ace-radar-view-scroll-indicator {
  position: absolute;
  left: 0;
  right: 0;
  border-radius: 4px;
  cursor: pointer;
  border-style: double;
  border-width: 3px;
}

.ace-radar-view-cursor-indicator {
  position: absolute;
  left: 0;
  right: 0;
  height: 4px;
  border-radius: 3px;
  cursor: pointer;
  border: 1px solid black;
}

.ace-radar-view-wrapper {
  position: relative;
  float: left;

  height: 100%;
  width: 6px;

  margin-right: 4px;
}
  </style>
