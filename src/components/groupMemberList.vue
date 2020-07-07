<template>
    <span v-if="this.permView==this.GROUP_INFO">
            <group-member-header></group-member-header>
            <group-member-line v-for="(member, index) in  groupMembers"
                               :key="index"
                               :name="member.name"
                               :email="member.email"
                               :id="member.id"
                               @memberSelected="memberSelected"

            ></group-member-line>
    </span>

</template>

<script>
    import GroupMemberLine from "./GroupMemberLine.vue";
    import GroupMemberHeader from "./GroupMemberHeader";
    import axios from "axios";

    export default {
        name: "groupMemberList",
        components: {GroupMemberLine, GroupMemberHeader},
        props:{
          groupId:{
              type: Number,
              required: true
          },
          permView:{
                type: Number,
                required: true
          }
        },
        data(){
            return {
                groupMembers: [],
                GROUP_INFO:1
            }
        },
        methods:{
            loadMembers(groupId){
                axios.get('http://localhost:8000/api/shan/groupMembers?XDEBUG_SESSION_START=14668', {
                    params:{
                        groupId: groupId
                    }
                })
                    .then(response => {
// eslint-disable-next-line no-debugger
                        // JSON responses are automatically parsed.
//                      debugger;
                        this.groupMembers = response.data;
//                        debugger;


                    })
                    .catch(e => {
                        this.errors.push(e);
                        console.log('groupMembers failed');
                    });
            }
        }
    }
</script>

<style scoped>

</style>
