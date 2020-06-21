<template>
    <span class="msgClass" v-if="msg.length>0">{{msg}}</span>
</template>

<script>
    export default {
        name: "menuMsg",
        props:{
            msg:{
                type: String,
                required: true
            }
        }
    }
</script>

<style scoped>
    .msgClass {
        font-family: Arial;
        font-size: 17px;
        padding: 1px;
        color: red;
    }

</style>
