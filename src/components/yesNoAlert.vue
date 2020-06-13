<template>
    <div class="ynClass">
        <div>{{message}}</div>
        <div> <button type="button" @click="yesClicked">Yes</button>  <button type="button" @click="noClicked">No</button> </div>
    </div>
</template>

<script>
    export default {
        name: "yesNoAlert",
        props:{
            message: {
                type: String,
                required: true
            }
        },
        methods:{
            yesClicked(){
                this.$emit('ynResult',['1']);
            },
            noClicked(){
                this.$emit('ynResult',['0']);
            }
        }
    }
</script>

<style scoped>
    .ynClass {
        width: 300px;
        height:100px;
        display: grid;
        grid-template-columns: 100%;
        grid-template-rows: 40px 40px;
        align-content: center;
        border: 2px solid blue;
        border-radius: 2px;
    }

</style>
