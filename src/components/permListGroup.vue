<template>
    <span>
        <a href="#" v-on:click="groupClicked" >{{this.groupDescription}}</a>
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
            }
        },
        methods:{
            groupClicked(){
                this.$emit('groupClicked',[this.groupId])
            }
        }
    }
</script>

<style scoped>

</style>
