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
                        :selectedId="selectedGroupId"
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
        <span v-if="this.view==this.FIND_MEMBER">
            <all-user-list :adminUserSelect="this.adminUserSelect" :allUserRefresh="this.allUserRefresh" :selectedId="this.selectedUserId" @userSelected="userSelected" @userUnSelected="userUnSelected" @userFound="userFound"></all-user-list>
        </span>
        <span v-if="this.view==this.ADD_MEMBER_TO_GROUP">

        </span>
        <span v-if="this.view==this.ORGANIZATION_GROUPS">
            <org-group-header></org-group-header>
            <div>
            <perm-list-group v-for="(org, index) in  OrganizationGroups"
                             :key="index"
                             :groupDescription="org.description"
                             :groupId="org.id"
                             :currentlySelectedGroupId="selectedGroupId"
                             :displayAsRow="true"
                             @groupClicked="orgGroupClicked"
            ></perm-list-group>
            </div>

        </span>

    </span>


</template>

<script>
    import PermListHeader from "./permListHeader.vue";
    import axios from "axios";
    import PermListLine from "./permListLine.vue";
    import GroupMemberLine from "./GroupMemberLine.vue";
    import GroupMemberHeader from "./GroupMemberHeader";
    import allUserList from "./allUserList";
    import permListGroup from "./permListGroup.vue";
    import orgGroupHeader from "./orgGroupHeader.vue";
    export default {
        name: "permList",
        components: {GroupMemberHeader, PermListLine, PermListHeader, GroupMemberLine, allUserList, permListGroup, orgGroupHeader},
        mounted(){
//            debugger;
            this.adminUserSelect = this.SELECT_USER;
            this.reloadLayoutPerms();
        },
        props:{
            layoutId:{
                type: Number,
                required: true
            }
        },
        methods:{
          setView(v){
            this.view=v;
          },
          setToMembers(){
              this.view=this.GROUP_INFO;
              this.$emit('groupMembersLoaded');
          },
          reloadLayoutPerms(){
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
          groupClicked(msg){
              console.log('group clicked', msg);
              this.$emit('showGroupMembers', [msg[0][0]]);
              console.log('got past the event');
              this.selectedGroupId=msg[0][0];
              this.groupMembers = this.getGroupMembers(msg[0], this.setGroupMembers);

          },
          orgGroupClicked(msg){
//              debugger;
              this.selectedGroupId=msg[0];
              this.$emit('orgGroupSelected', this.selectedGroupId );
          },
          setGroupMembers(groupMembers){
//              debugger;
              this.groupMembers=groupMembers;
              this.setToMembers();
          },
          getGroupMembers(groupId, setGm){
//              debugger;
              axios.get('http://localhost:8000/api/shan/groupMembers?XDEBUG_SESSION_START=14668', {
                  params:{
                      groupId: groupId
                  }
              })
                  .then(response => {
// eslint-disable-next-line no-debugger
                      // JSON responses are automatically parsed.
//                      debugger;
                      setGm(response.data);


                  })
                  .catch(e => {
                      this.errors.push(e);
                      console.log('groupMembers failed');
                  });
          },
          showOrganizationGroups(orgId){
//              debugger;
              this.getOrgGroups(orgId, this.setOrgGroups);
          },
          setOrgGroups(orgGroups){
            this.OrganizationGroups = orgGroups;
            this.setView(this.ORGANIZATION_GROUPS);
          },
          getOrgGroups(orgId, setOg){
              debugger;
              axios.get('http://localhost:8000/api/shan/orgGroups?XDEBUG_SESSION_START=14668', {
                  params:{
                      orgId: orgId,
                      layoutId:this.layoutId
                  }
              })
                  .then(response => {
// eslint-disable-next-line no-debugger
                      // JSON responses are automatically parsed.
                      debugger;
                      setOg(response.data);


                  })
                  .catch(e => {
                      this.errors.push(e);
                      console.log('groupMembers failed');
                  });
          },
          memberSelected(msg){
              console.log("member selected", msg);
          },
          showPerms(){
              this.view=this.PERMS;
 //             this.$emit("showPerms");
          },
          showAllUsers(){
              this.view=this.ADD_MEMBER_TO_GROUP;
          }
        },
        data (){
            return {
                PERMS:0,
                GROUP_INFO:1,
                NEW_GROUP:2,
                FIND_MEMBER:3,
                ADD_NEW_MEMBER:4,
                ADD_USER_TO_GROUP:5,
                ORGANIZATION_GROUPS:6,

                NEW_USER:1,
                SELECT_USER:0,

                currentPerms: [],
                groupMembers: [],
                OrganizationGroups: [],
                adminUserSelect:0,
                allUserRefresh:0,
                selectedGroupId:0,
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
