<template>
    <div class="dialogComponent" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <span class="headingText">Who Can Access This Space ?</span>
        </div>
        <div class="dialogComponentBody">
            <perm-list :permViewStatus="this.permViewStatus" :layoutId="layoutId" ></perm-list>

        </div>
        <div class="dialogComponentFooter">
            <a href="#" class="linkStyle" v-on:click="cancelClicked" >Cancel</a>
        </div>
    </div>
</template>

<script>
    import PermList from "./permList";
    export default {
        name: "permSetter",
        components: {PermList},
        mounted(){
            this.permViewStatus = this.PERMS;
        },
        props: {
          layoutId:{
              type: Number,
              required: true
          }
        },
        data(){
            return {
                PERMS:0,
                GROUP_INFO:1,
                NEW_GROUP:2,
                permViewStatus:0
            }
        },
        methods:{
            cancelClicked(){
                this.$emit('configSelected',['permSetterCanceled']);
            }
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
    .linkStyle{
        font-family: Arial;
        font-size: medium;
        color: #0a3aff;
    }
</style>
