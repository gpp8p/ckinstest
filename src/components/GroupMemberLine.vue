<template>
    <span class="lline" v-on:click="this.memberSelected"><span class="gitem">{{name}}</span><span class="gitem">{{email}}</span></span>
</template>

<script>
    export default {
        name: "GroupMemberLine",
        props:{
            name:{
                type: String,
                required: true
            },
            email:{
                type: String,
                required: true
            },
            id:{
                type: Number,
                required: true
            }
        },
        methods:{
            memberSelected(){
                this.$emit('memberSelected', [this.id]);
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

    }
</style>
