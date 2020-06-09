<template>
    <div class="newLayoutComponent" ref="drg"  v-if="status==this.NEW_LAYOUT" draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >

        <div class="dialogComponentBody">
            <span class="inputPrompt">Layout Name:</span><span><input ref="name" type="text" size="30" v-model="name" @keydown.tab.exact = "nameCheck" required = true /></span>
            <span class="inputPrompt">Layout Description:</span><span><input ref="description" type="text" size="50" v-model="description" @keydown.tab.exact = "descriptionCheck"/></span>
            <span class="inputPrompt">Rows:</span><span><input ref="rows" type="text" size="5" v-model="rows" @keydown.tab.exact = "rowCheck"/></span>
            <span class="inputPrompt">Columns:</span><span><input ref="cols" type="text" size="5" v-model="cols" @keydown.tab.exact = "colsCheck"/></span>
            <span class="inputPrompt">Background Color:</span>
            <span class="input-color-container">
                    <input  id="input-color" type="color" class="input-color" @change="newColor" :value="colorVal"/>
            </span>

        </div>
        <div class="newLayoutFooter">
            <a href="#" class="linkStyle" v-on:click="saveClicked" >Save</a>
            <a href="#" class="linkStyle" v-on:click="cancelClicked" >Cancel</a>
        </div>
    </div>
</template>

<script>
    export default {
        name: "NewLinkLayout",
        props:{
            status:{
                type: Number,
                required:true
            }
        },
        data(){
            return {
                EXISTING_LAYOUTS:0,
                NEW_LAYOUT:1,
            }
        }
    }
</script>

<style scoped>

    .newLayoutComponent {
        height:85%;
        width:800px;
        background-color: #ab97ff;
        margin-top: 2%;
    }
    .headingText{
        margin-top: 5px;
    }
    .dialogComponentBody {
        height: 80%;
        margin-left: 10px;
        margin-right: 10px;
        display: grid;
        grid-template-columns: 30% 70%;
        grid-template-rows: 15% 15% 15% 15%


    }
    .newLayoutFooter {
        height: 15%;
        margin-left: 10px;
        margin-right: 10px;
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
    .highlight {
        background-color: #feff06;
        margin-left: 10px;
        margin-right: 10px;
        color: blue;
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;

    }

    .inputPrompt {
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;
    }
    .input-color-container {
        position: relative;
        overflow: hidden;
        width: 15px;
        height: 20px;
        top: 2px;
        margin-right: 5px;
        border: solid 2px #ddd;
        border-radius: 5px;
    }

    .input-color {
        position: absolute;
        right: -8px;
        top: -5px;
        width: 36px;
        height: 26px;
        border: none;
    }

    .input-color-label {
        cursor: pointer;
        text-decoration: underline;
        color: #dbd50c;
        margin-right: 30px;
    }
    .colorSpan{
        margin-left: 26px;
    }
    .linkHelperStyle {
        width: 85%;
        overflow: auto;
        height:75%;
        margin-left: 0px;
    }
    .cancelLink {
        margin-left: 20%;
    }

</style>
