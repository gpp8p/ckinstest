<template>
    <span class="permListContainer">
     <span v-if="this.view==this.PERMS" >
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
                        @groupClicked="groupClicked"></perm-list-line>

    </span>
    <span v-if="this.view==this.GROUP_INFO">
        <group-member-header></group-member-header>
        <group-member-line v-for="(member, index) in  groupMembers"
                           :key="index"
                           :name="member.name"
                           :email="member.email"
                           :id="member.id"
                           @memberSelected="memberSelected"

        ></group-member-line>
    </span>
    </span>

</template>

<script>
    import PermListHeader from "./permListHeader.vue";
    import axios from "axios";
    import PermListLine from "./permListLine.vue";
    import GroupMemberLine from "./GroupMemberLine.vue";
    import GroupMemberHeader from "./GroupMemberHeader";
    export default {
        name: "permList",
        components: {GroupMemberHeader, PermListLine, PermListHeader, GroupMemberLine},
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
            layoutId:{
                type: Number,
                required: true
            }
        },
        methods:{
          groupClicked(msg){
              console.log('group clicked', msg);
              this.groupMembers = this.getGroupMembers(msg[0], this.setGroupMembers);

          },
          setGroupMembers(groupMembers){
              debugger;
              this.groupMembers=groupMembers;
              this.view=this.GROUP_INFO;
//              this.$emit('showGroupMembers');
          },
          getGroupMembers(groupId, setGm){
              debugger;
              axios.get('http://localhost:8000/api/shan/groupMembers?XDEBUG_SESSION_START=14668', {
                  params:{
                      groupId: groupId
                  }
              })
                  .then(response => {
// eslint-disable-next-line no-debugger
                      // JSON responses are automatically parsed.
                      debugger;
                      setGm(response.data);

                  })
                  .catch(e => {
                      this.errors.push(e);
                      console.log('groujpMembers failed');
                  });
          },
          memberSelected(msg){
              console.log("member selected", msg);
          }
        },
        data (){
            return {
                PERMS:0,
                GROUP_INFO:1,
                NEW_GROUP:2,
                currentPerms: [],
                groupMembers: [],
                view:0
            }
        }
    }
</script>

<style scoped>
    .permListContainer {
        width: 450px;
    }

</style>
