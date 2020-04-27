<template>
    <div class="dialogComponent" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <span class="headingText">New Card</span>
        </div>
        <br/>

        <div class="dialogComponentBody">
            <span class="inputPrompt">Card Name:</span><span><input ref="name" type="text" size="30" v-model="name" @keydown.tab.exact = "nameCheck" required = true /></span>
            <span class="inputPrompt">Card Type:</span>
            <span>
                <select ref="cardComponentSelect" @change="cardSelectionMade($event)">
                    <option value="select">Select Card Type</option>
                    <option  value="blankComponent">Blue Card</option>
                    <option  value="greenComponent">Green Card</option>
                    <option  value="cktestComponent">CK-Editor Card</option>
                    <option  value="textShow">Text Card</option>
                </select>
            </span>
        </div>
        <div class="dialogComponentFooter">
            <a href="#" class="linkStyle" v-on:click="saveClicked" >Save</a>
            <a href="#" class="linkStyle" v-on:click="cancelClicked" >Cancel</a>
        </div>
    </div>


</template>

<script>
    import Vue from 'vue';
//    import axios from "axios";
    export default {
        name: "SimpleNewCard",
        data () {
            return {
                name:'',
                type:''
            }
        },
        mounted(){
            let self = this
            Vue.nextTick()
                .then(function () {
                    console.log(self.$refs.name.focus())
                })        },
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
            nameCheck(event) {
//                debugger;
                if (this.name.length == 0) {
                    alert('This field is required');
                    event.preventDefault();
                }
            },
            cardSelectionMade(evt){
                console.log('Card type selection made:'+evt.target.value);
                this.type = evt.target.value;
            },

            cancelClicked(){
                this.$emit('configSelected',['cancel']);
            },
            saveClicked(){
                //        debugger;
                this.$emit('saveNewCard', [this.name, this.type]);
            },

        }
    }
</script>

<style scoped>

    .dialogComponent {
        height:250px;
        width:500px;
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
        height: 10%;
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




</style>