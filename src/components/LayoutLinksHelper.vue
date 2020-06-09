<template>
    <div class="dialogComponent" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <span class="headingText">Click on Page Your Wish to Link To:</span>
            <span class="cancelLink"><a href="#" class="linkStyle" v-on:click="newLayout" >Link to a New Layout</a></span>
            <span class="cancelLink"><a href="#" class="linkStyle" v-on:click="cancelClicked" >Cancel</a></span>
        </div>
        <br/>

        <div class="linkHelperStyle">
            <layout-links :status="linkHelperStatus"  @layoutSelected="layoutSelected"></layout-links>
            <NewLinkLayout :status="linkHelperStatus"  ></NewLinkLayout>
        </div>

    </div>


</template>

<script>

//    import axios from "axios";
import layoutLinks from "../components/LayoutLinks.vue"
import NewLinkLayout from "../components/NewLinkLayout.vue"

    export default {
        name: "LayoutLinksHelper",
        components: {layoutLinks, NewLinkLayout},
        data(){
            return {
                EXISTING_LAYOUTS:0,
                NEW_LAYOUT:1,
                linkHelperStatus:this.EXISTING_LAYOUTS
            }
        },
        mounted(){
          this.linkHelperStatus = this.EXISTING_LAYOUTS;
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
  //              debugger;
                this.$emit('configSelected',['layoutLinkHelperCanceled']);
            },
            saveClicked(){
                //        debugger;
                this.$emit('saveNewCard', [this.name, this.type]);
            },
            layoutSelected(msg){
                this.$emit('layoutSelected', [msg[0]]);
            },
            newLayout(){
                this.linkHelperStatus=this.NEW_LAYOUT;
            }

        }
    }
</script>

<style scoped>

    .dialogComponent {
        height:300px;
        width:800px;
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
    .headingText{
        margin-top: 5px;
    }
    .dialogComponentBody {
        height: 72%;
        margin-left: 10px;
        margin-right: 10px;
        display: grid;
        grid-template-columns: 30% 70%;
        grid-template-rows: 15% 15% 15% 15%


    }
    .dialogComponentFooter {
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
