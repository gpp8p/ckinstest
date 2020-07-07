<template>
    <span v-if="this.permView==this.PERMS" class="permListContainer">
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
                        :opt3Value="perm.opt3"
                        @groupClicked="groupClicked">

        </perm-list-line>
    </span>

</template>

<script>
    import PermListHeader from "./permListHeader.vue";
    import axios from "axios";
    import PermListLine from "./permListLine.vue";
    export default {
        name: "permList2",
        components: {PermListHeader, PermListLine},
        mounted(){
//            debugger;
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
                    //                   debugger;
                    this.currentPerms = response.data;
                })
                .catch(e => {
                    this.errors.push(e);
                    console.log('viewableLayouts failed');
                });
        },
        props:{
            layoutId:{
                type: Number,
                required: true
            },
            permView:{
                type: Number,
                required: true
            },
            selectedId:{
                type: Number,
                required: true
            }
        },
        data (){
            return {
                PERMS:0,
                currentPerms: [],
                selectedGroupId:0
            }
        },
        methods:{
            groupClicked(msg){
                this.selectedGroupId = msg[0][0];
                this.$emit('groupClicked', [msg[0][0]]);
            }
        }

    }
</script>

<style scoped>
    .permListContainer {
        width: 450px;
    }
</style>
