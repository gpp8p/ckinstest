<template>
    <span class="lline" v-on:click="this.orgUserSelected"><span class="gitem">{{email}}</span><span class="gitem">{{name}}</span></span>
</template>

<script>
    export default {
        name: "orgUserListLine",
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
            }
        },
        methods:{
            orgUserSelected(){
                this.$emit('orgUserSelected', [this.id, this.email, this.name]);
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
    .gitem {
        text-align: left;
        margin-left: 5px;
    }
</style>
