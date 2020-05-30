<template>
    <div class="dialogComponent" ref="drg"  draggable="false"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <a href="#" class="linkStyle" v-on:click="cancelClicked" >Cancel</a>
            <a v-if="this.viewStatus==this.VIEW_CKEDITOR" href="#" class="linkStyle" v-on:click="linkClicked" >Link to Another Page</a>
            <a href="#" class="linkStyle" v-on:click="saveClicked" >Save</a>
        </div>
        <br/>

        <div  v-if="this.viewStatus==this.VIEW_CKEDITOR" class="dialogComponentBody">
            <ckeditor  :editor="editor" v-model="editorData" :config="editorConfig" @ready="onEditorReady" ></ckeditor>
        </div>
    </div>


</template>

<script>
    import ClassicEditor from '@ckeditor/ckeditor5-editor-classic/src/classiceditor';

    import EssentialsPlugin from '@ckeditor/ckeditor5-essentials/src/essentials';
    import BoldPlugin from '@ckeditor/ckeditor5-basic-styles/src/bold';
    import ItalicPlugin from '@ckeditor/ckeditor5-basic-styles/src/italic';
    import LinkPlugin from '@ckeditor/ckeditor5-link/src/link';
    import ParagraphPlugin from '@ckeditor/ckeditor5-paragraph/src/paragraph';

    import Image from '@ckeditor/ckeditor5-image/src/image';
    import ImageToolbar from '@ckeditor/ckeditor5-image/src/imagetoolbar';
//    import ImageCaption from '@ckeditor/ckeditor5-image/src/imagecaption';
    import ImageStyle from '@ckeditor/ckeditor5-image/src/imagestyle';
    import ImageResize from '@ckeditor/ckeditor5-image/src/imageresize';
    import EasyImage from '@ckeditor/ckeditor5-easy-image/src/easyimage';
    import SimpleUploadAdapter from '@ckeditor/ckeditor5-upload/src/adapters/simpleuploadadapter';

    import Font from '@ckeditor/ckeditor5-font/src/font'
    import Alignment from '@ckeditor/ckeditor5-alignment/src/alignment'
    import List from '@ckeditor/ckeditor5-list/src/list'
    import TodoList from '@ckeditor/ckeditor5-list/src/todolist';
    import Table from '@ckeditor/ckeditor5-table/src/table';
    import TableToolbar from '@ckeditor/ckeditor5-table/src/tabletoolbar';
    import PasteFromOffice from '@ckeditor/ckeditor5-paste-from-office/src/pastefromoffice'

    import CKEditorInspector from '@ckeditor/ckeditor5-inspector';


    //    import axios from "axios";
    export default {
        name: "SimpleCkEditor",
        mounted(){
          this.viewStatus=this.VIEW_CKEDITOR;
        },
        props:{
          updateCallback :{
              type: Function,
              required: true
          },
          cardData: {
              type: String,
              required: true
          },
          layoutLink: {
              type: String,
              required: true
          }
        },
        watch: {
            layoutLink: function(){
  //              debugger;

                this.forwardToUrl = "http://localhost:8080/displayLayout/"+this.layoutLink;
                this.editorInUse.execute( 'link', this.forwardToUrl );
            }
        },
        data() {
            return {
                editor: ClassicEditor,
                editorDisabled: false,
                editorConfig: {
                    plugins: [
                        EssentialsPlugin,
                        BoldPlugin,
                        ItalicPlugin,
                        LinkPlugin,
                        ParagraphPlugin,
                        Image,
                        ImageToolbar,
//                        ImageCaption,
                        ImageStyle,
                        ImageResize,
                        EasyImage,
                        SimpleUploadAdapter,
                        Font,
                        Alignment,
                        List,
                        TodoList,
                        Table,
                        TableToolbar,
                        PasteFromOffice
                    ],

                    toolbar: {
                        items: [
                            'bold',
                            'italic',
                            'link',
                            'undo',
                            'redo',
                            'imageUpload',
                            'fontSize',
                            'fontFamily',
                            'fontColor',
                            'fontBackgroundColor',
                            'alignment',
                            'bulletedList',
                            'numberedList',
                            'todoList',
                            'insertTable'


                        ]
                    },
                    image: {
                        toolbar: [ 'imageTextAlternative', '|', 'imageStyle:alignLeft', 'imageStyle:full', 'imageStyle:alignRight' ],

                        styles: [
                            // This option is equal to a situation where no style is applied.
                            'full',

                            // This represents an image aligned to the left.
                            'alignLeft',

                            // This represents an image aligned to the right.
                            'alignRight'
                        ]
                    },
                    simpleUpload: {
                        uploadUrl: 'http://localhost:8000/imageUploadCk?XDEBUG_SESSION_START=19403'
                    },
                    fontFamily: {
                        options: [
                            'default', 'Helvetica', 'Times', 'Verdana', 'Arial Narrow', 'Candara', 'Geneva', 'Calibri', 'Optima', 'Cambria', 'Garamond', 'Perpetua', 'Monaco', 'Didot', 'Brush Script MT', 'Lucida Bright', 'Copperplate',
                            'Ubuntu, Arial, sans-serif',
                            'Ubuntu Mono, Courier New, Courier, monospace'
                        ]
                    },
                    alignment: {
                        options: [ 'left', 'right', 'center', 'justify' ]
                    },
                    table: {
                        contentToolbar: [ 'tableColumn', 'tableRow', 'mergeTableCells' ]
                    }

                },
                editorData:this.cardData,
                editorInUse:{},
                VIEW_CKEDITOR:0,
                VIEW_LAYOUT_LIST:1,
                viewStatus:this.VIEW_CKEDITOR,
                forwardToUrl:''
            };
        },
        methods: {
            handleDragStart(evt){
                this.firstMouseX = evt.screenX;
                this.firstMouseY = evt.screenY;
                this.$emit('startDrag', [this.firstMouseX,this.firstMouseY]);
            },
            handleDragEnd(evt){
                // eslint-disable-next-line no-debugger
                //               debugger;
                this.lastMouseX = evt.screenX;
                this.lastMouseY = evt.screenY;
                this.$emit('newLocation',[this.lastMouseX, this.lastMouseY]);
                // eslint-disable-next-line no-console
                console.log(evt);
            },
            cancelClicked(){
                this.$emit('configSelected',['cancel']);
            },
            saveClicked(){
                debugger;
                this.updateCallback(this.editorData, 'cardText');
                this.$emit('configSelected', ['hideCkDialog']);
                this.updateCallback(this.editorData, 'saveCardContent');
            },
            linkClicked(){
                this.$emit('configSelected', ['layoutLinkHelperRequested'])
            },
            linkCancelClicked(){
                this.viewStatus=this.VIEW_CKEDITOR;
            },
            onEditorReady(editor){
                this.editorInUse = editor;
                CKEditorInspector.attach( this.editorInUse );
            }

        }
    }
</script>
<style>
    .ck-editor__editable {
        min-height: 350px;
        max-height:350px;
        min-width:950px;
    }
    .image {
        display: table;
        clear: both;
        text-align: center;
    }
    .image-style-side {
        float: right;

    }
    .image-style-side > img {
        max-width: 100%;
        height: auto;
    }
    .image-style-align-right {
        float: right;
    }
    .image-style-align-left {
        float: left;
    }


</style>

<style scoped>
    .dialogComponent {
        height:500px;
        width:1000px;
        background-color: #ab97ff;
        border: 2px solid blue;
        border-radius: 8px;
        box-shadow: 10px 10px 5px grey;
    }
    .dialogComponentHeader {
        height:10%;
        background-color: #fff722;
        border-top-left-radius: 8px;
        border-top-right-radius: 8px;
        text-align: center;
        color: blue;
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;
    }

    .dialogComponentBody {
        height: 72%;
        margin-left: 10px;
        margin-right: 10px;
        display: grid;
        grid-template-columns: 30% 70%;
        grid-template-rows: 15% 15% 15% 15%


    }
    .linkStyle {
        margin-left: 10px;
        margin-right: 10px;
        color: blue;
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;

    }


</style>
