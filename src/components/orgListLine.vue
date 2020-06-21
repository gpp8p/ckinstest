<template>
    <span class="lline" v-on:click="this.orgSelected"><span class="gitem">{{org_label}}</span><span class="gitem">{{description}}</span></span>
</template>

<script>
    export default {
        name: "orgListLine",
        props:
            {
                id: {
                    type: String,
                    required: true
                },
                org_label: {
                    type: String,
                    required: true
                },
                description: {
                    type: String,
                    required: true
                },
            },
        methods:{
            orgSelected(){
                this.$emit('orgSelected', [this.id, this.org_label]);
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
