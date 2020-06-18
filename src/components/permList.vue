<template>
    <span v-if="permViewStatus==this.PERMS" class="permListContainer">
        <perm-list-header></perm-list-header>
        <perm-list-line v-for="(perm, index) in currentPerms "
                        :key="index"
                        :groupId="perm.id"
                        :layoutId="layoutId"
                        :groupDescription="perm.description"
                        :viewValue="perm.view"
                        :authorValue="perm.author"
                        :adminValue="perm.admin"
                        :opt1Value="perm.opt1"
                        :opt2Value="perm.opt2"
                        :opt3Value="perm.opt3" ></perm-list-line>

    </span>
</template>

<script>
    import PermListHeader from "./permListHeader.vue";
    import axios from "axios";
    import PermListLine from "./permListLine.vue";
    export default {
        name: "permList",
        components: {PermListLine, PermListHeader},
        mounted(){
            debugger;
            axios.get('http://localhost:8000/api/shan/layoutPerms?XDEBUG_SESSION_START=14668', {
                params:{
                    orgId:this.$store.getters.getOrgId,
                    userId:this.$store.getters.getLoggedInUserId,
                    layoutId:this.layoutId
                }
            })
                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    debugger;
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
    .permListContainer {
        width: 450px;
    }

</style>
