<template>
    <span v-on:click="groupClicked" :class="this.lineClass">
        {{this.groupDescription}}
    </span>
</template>

<script>
    export default {
        name: "permListGroup",
        props:{
            groupId:{
                type: Number,
                required: true
            },
            groupDescription:{
                type: String,
                required: true
            },
            currentlySelectedGroupId:{
                type: Number,
                required: true
            }
        },
        watch: {
            currentlySelectedGroupId: function(){
                debugger;
                this.showSelected();
            }
        },
        methods:{
            groupClicked(){
                this.$emit('groupClicked',[this.groupId])
            },
            showSelected(){
                if(this.groupId==this.currentlySelectedGroupId){
                    this.lineClass='llineSelected';
                }else{
                    this.lineClass='lline';
                }
            }
        },
        data(){
            return {
                lineClass: "lline"
            }
        }
    }
</script>

<style scoped>
    .lline {
        font-size: medium;
        font-family: Arial;
        align-items: left;
    }
    .lline:hover {
        background-color: burlywood;
    }
    .llineSelected {
        font-size: medium;
        font-family: Arial;
        align-items: left;
        background-color: #ff330a;
    }
    .lline:hover {
        background-color: burlywood;
    }
    .gitem {
        text-align: left;
        margin-left: 5px;
    }
</style>
