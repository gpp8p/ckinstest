<template>
    <span :class="this.lineClass"  v-on:click="this.orgUserSelected"><span class="gitem">{{name}}</span><span class="gitem">{{email}}</span></span>
</template>

<script>
    export default {
        name: "allUserLine",
        props:{
            id: {
                type: String,
                required: true
            },
            name: {
                type: String,
                required: true
            },
            email: {
                type: String,
                required: true
            },
            selectedId:{
                type: Number,
                required: true
            }
        },
        watch: {
            selectedId: function(){
                this.showSelected();
            }
        },
        mounted(){
            this.showSelected();
        },
        methods:{
            orgUserSelected(){
                if(this.isSelected){
                    this.isSelected=false;
//                    this.lineClass='lline';
                    this.$emit('userUnSelected', [this.id, this.email, this.name]);
                }else{
                    this.isSelected=true;
//                    this.lineClass='llineSelected';
                    this.$emit('userSelected', [this.id, this.email, this.name]);
                }
            },
            showSelected(){
                if(this.id==this.selectedId){
                    this.lineClass='llineSelected';
                }else{
                    this.lineClass='lline';
                }
            }
        },
        data(){
            return {
                isSelected:false,
                lineClass: "lline"
            }
        }
    }
</script>

<style scoped>
    .lline {
        display: grid;
        grid-template-columns: 30% 70%;
        font-size: medium;
        font-family: Arial;
        align-items: left;
    }
    .lline:hover {
        background-color: burlywood;
    }
    .llineSelected {
        display: grid;
        grid-template-columns: 30% 70%;
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
