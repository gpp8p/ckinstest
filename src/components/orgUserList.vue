<template>
        <span>
       <orgUserListHeader></orgUserListHeader>
                <orgUserListLine v-for="(orgUser, index) in allOrgUsers"
                             :key="index"
                             :id="orgUser.id"
                             :name="orgUser.name"
                             :email="orgUser.email"
                             @orgUserSelected="orgUserSelected"
                >
                </orgUserListLine>

    </span>
</template>

<script>
    import orgUserListHeader from "./orgUserListHeader";
    import orgUserListLine from "./orgUserListLine";
    import axios from "axios";

    export default {
        name: "orgUserList",
        components: {orgUserListHeader, orgUserListLine},
        props:{
          orgId:{
              type: Number,
              required: true
          }
        },
        mounted: function() {
            console.log('mounted runs in  orgUserList');
//            axios.get('http://localhost:8000//layoutList')
            axios.get('http://localhost:8000/api/shan/orgUsers?XDEBUG_SESSION_START=19181',
            {
                params:{
                    orgId:this.orgId
                }
            })

                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    this.allOrgUsers = response.data;
                    console.log('successful return from orgUsers');
                })
                .catch(e => {
                    console.log(e);
                    this.errors.push(e);
                });
        },
        data(){
            return{
                allOrgUsers:[]
            }
        },
        methods:{
            orgSelected(msg){
                this.$emit('orgUserSelected',[msg]);
                console.log('orgUsersSelected summoned:'+msg);
            }
        }


    }
</script>

<style scoped>

</style>
