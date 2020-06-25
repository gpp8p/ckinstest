<template>
    <div class="dialogComponent" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <span class="headingText">New Organization</span>
        </div>
        <br/>

        <div class="dialogComponentBody">
            <span class="inputPrompt">Organization Name:</span><span><input ref="name" type="text" size="30" v-model="name" @keydown.tab.exact = "nameCheck" required = true /></span>
            <span class="inputPrompt">Organization Description:</span><span><input ref="description" type="text" size="50" v-model="description" @keydown.tab.exact = "descriptionCheck"/></span>
            <span class="inputPrompt selectAdminUser">Homepage</span>
            <span class="inputPrompt">Rows:</span><span><input ref="rows" type="text" size="5" v-model="rows" @keydown.tab.exact = "rowCheck"/></span>
            <span class="inputPrompt">Columns:</span><span><input ref="cols" type="text" size="5" v-model="cols" @keydown.tab.exact = "colsCheck"/></span>

            <span class="inputPrompt selectAdminUser">
                <span class="cinput">
                    <span class="inputPrompt"><input type="radio" name="backgroundType" value="color"  @change="colorSelected"  checked="true"/>Color:</span>
                    <span class="input-color-container" v-if="this.showBack==this.SHOW_COLOR">
                        <input id="input-color" type="color" class="input-color" @change="newColor" :value="colorVal"/>
                    </span>
                </span>

            </span>
            <span class="inputPrompt selectAdminUser">
                <span class="fileUpload">
                     <span><input type="radio" name="backgroundType" value="image"  @change="imageSelected"/>Image:</span>
                    <span v-if="this.showBack==this.SHOW_IMAGE">
                        <input  type="file" id="file" ref="file" v-on:change="handleFileUpload()"/>
                        <a v-if="this.imageIdentified==true" href="#" @click="uploadImageFile">Upload this image</a>
                    </span>
                </span>
            </span>
            <div class="selectAdminUser">
                <span class="inputPrompt"><input type="radio" name="adminUserSelectType" value="select" checked="true" @click="selectAdmin">Select Space Admin<input type="radio" name="adminUserSelectType" value="new" @click="newAdmin">Admin is New User</span>
                <new-user-entry :adminUserSelect="this.adminUserSelect"></new-user-entry>
                <all-user-list :adminUserSelect="this.adminUserSelect"></all-user-list>
            </div>
        </div>

        <div class="dialogComponentFooter">
            <a href="#" class="linkStyle" v-on:click="saveClicked" >Save</a>
            <a href="#" class="linkStyle" v-on:click="cancelClicked" >Cancel</a>
        </div>
    </div>

</template>

<script>
    import Vue from "vue";
    import axios from "axios";
    import newUserEntry from "../components/newUserEntry.vue";
    import allUserList from "../components/allUserList.vue";

    export default {
        name: "SimpleNewOrg",
        components:{newUserEntry, allUserList},
        data () {
            return {
                name:'',
                description:'',
                rows:'',
                cols:'',
                colorVal:'#dbddd0',
                layoutId:0,
                adminUserId:0,
                adminUserSelect:0,
                NEW_USER:1,
                SELECT_USER:0,
                backType:'',
                showBack:0,
                SHOW_COLOR:0,
                SHOW_IMAGE:1,
                imageIdentified: false


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
            colorSelected(){
                console.log('color selected');
              this.showBack=this.SHOW_COLOR;
            },
            imageSelected(){
                console.log('image selected');
                this.showBack=this.SHOW_IMAGE;
            },
            handleFileUpload(){
                this.imageIdentified=true;
            },
            nameCheck(event) {
//                debugger;
                if (this.name.length == 0) {
                    alert('This field is required');
                    event.preventDefault();
                }
            },

            descriptionCheck(event){
                if (this.description.length == 0) {
                    alert('This field is required');
                    event.preventDefault();
                }
            },
            rowCheck(event){
                if (this.rows.length == 0) {
                    alert('This field is required');
                    event.preventDefault();
                }
                if(isNaN(this.rows)){
                    alert('a number is required');
                    event.preventDefault();
                }

            },
            colsCheck(event){
                if (this.cols.length == 0) {
                    alert('This field is required');
                    event.preventDefault();
                }
                if(isNaN(this.cols)){
                    alert('a number is required');
                    event.preventDefault();
                }

            },
            selectAdmin(){
              this.adminUserSelect = this.SELECT_USER;
            },
            newAdmin(){
                this.adminUserSelect = this.NEW_USER;
            },
            cancelClicked(){
                this.$emit('configSelected',['cancel']);
            },
            newColor(evt){
                this.colorVal=evt.target.value;
            },
            saveClicked(){
//        debugger;
                axios.post('http://localhost:8000/api/newOrg?XDEBUG_SESSION_START=17516', {
                    name: this.name,
                    description: this.description,
                    height: this.rows,
                    width: this.cols,
                    backgroundColor: this.colorVal,
                    adminUserId:this.adminUserId
                }).then(response=>
                {
//            debugger;
                    this.layoutId=response.data;
                    this.$emit('saveNewLayout', [this.layoutId, this.height, this.width, this.description, this.name, this.backgroundColor]);
//                this.$refs.editGrid.createBlankLayout(msg[2],msg[3],msg[1],msg[0]);
                }).catch(function(error) {
                    console.log(error);
                });
            },

        }
    }
</script>

<style scoped>

    .dialogComponent {
        height:550px;
        width:500px;
        background-color: #ab97ff;
        border: 2px solid blue;
        border-radius: 8px;
        box-shadow: 10px 10px 5px grey;
    }
    .dialogComponentHeader {
        height:8%;
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
        grid-template-rows: 10% 12% 8% 8% 8% 8% 8%


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
    .cinput {
        display: grid;
        grid-template-columns: 30% 70%;
    }
    .colorSpan{
        margin-right: 26px;
    }
    .selectAdminUser {
        grid-column: 1/-1;
    }
    .fileUpload {
        display: grid;
        grid-template-columns: 30% 70%;
    }

</style>
