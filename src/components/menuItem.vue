<template>
    <span class="layoutMenuItem" @click="itemClicked">
        {{itemLabel}}
    </span>
</template>

<script>
    export default {
        name: "menuItem",
        props:{
            itemLabel:{
                type: String,
                required: true
            }
        },
        methods:{
            itemClicked(){
                this.$emit('menuSelection', [this.itemLabel]);
            }
        }
    }
</script>

<style scoped>
    .layoutMenuItem {
        background-color: #ffcd90;
        font-family: Arial;
        font-size: 17px;
        padding: 1px;
    }
    .layoutMenuItem:hover {
        background-color: #fff722;
        color:red;
    }

    
</style>