<template>
    <span>
        <div class="configComponentHeader">
            <span class="headingText"><MyClickLink @myButtonClicked="pageLinkClicked" buttonLabel="Page Link Helper"></MyClickLink></span>
        </div>
        <div id="ckeditorComponent" class="ckdiv" >
            <ckeditor :editor="editor" v-model="editorData" :config="editorConfig"></ckeditor>
        </div>
    </span>
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
    import ImageCaption from '@ckeditor/ckeditor5-image/src/imagecaption';
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


    export default {
        name: "ckeditorComponent",
        props: {
            cardStyle: {
                type: String,
                required: true
            },
            cardId: {
                type: String,
                required: true
            },
            cardKey: {
                type: String,
                required: true
            },
            cardPosition: {
                type: Array,
                required: true
            },
            cardProperties: {
                type: String,
                required: false
            }
        },
        data() {
            return {
                editor: ClassicEditor,
                editorData: '<p>Content of the editor.</p>',
                editorConfig: {
                    plugins: [
                        EssentialsPlugin,
                        BoldPlugin,
                        ItalicPlugin,
                        LinkPlugin,
                        ParagraphPlugin,
                        Image,
                        ImageToolbar,
                        ImageCaption,
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
                        toolbar: [ 'imageTextAlternative', '|', 'imageStyle:full', 'imageStyle:side', 'imageStyle:alignLeft', 'imageStyle:alignCenter', 'imageStyle:alignRight' ]
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

                }
            };
        }
    }
</script>

<style scoped>
    .ckdiv {
        width: 100%;
    }

</style>