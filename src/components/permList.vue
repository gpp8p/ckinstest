<template>
    <span v-if="permViewStatus==this.PERMS">
        <perm-list-header></perm-list-header>
        <perm-list-line></perm-list-line>

    </span>
</template>

<script>
    import PermListHeader from "./permListHeader";
    import PermListLine from "./permListLine";
    import axios from "axios";
    export default {
        name: "permList",
        components: {PermListLine, PermListHeader},
        mounted(){
            axios.get('http://localhost:8000/api/shan/layoutPerms', {
                params:{
                    orgId:this.$store.getters.getOrgId,
                    userId:this.$store.getters.getLoggedInUserId,
                    layoutId:this.layoutId
                }
            })
                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    this.currentPerms = response.data;
                })
                .catch(e => {
                    this.errors.push(e);
                    console.log('viewableLayouts failed');
                });
        },
        props:{
            permViewStatus:{
                type: Number,
                required: true
            },
            layoutId:{
                type: Number,
                required: true
            }
        },
        data (){
            return {
                PERMS:0,
                GROUP_INFO:1,
                NEW_GROUP:2,
                currentPerms: []
            }
        }
    }
</script>

<style scoped>

</style>
