<template>
        <span v-if="adminUserSelect==this.SELECT_USER" class="allUserContainer">
            <allUserHeader></allUserHeader>
                <allUserLine v-for="(user, index) in this.users"
                                 :key="index"
                                 :id="user.id"
                                 :name="user.name"
                                 :email="user.email"
                                 @userSelected="userSelected"
                                 @userUnselected="userUnSelected"
                >
                </allUserLine>

    </span>
</template>

<script>
    import allUserHeader from "../components/allUserHeader.vue";
    import allUserLine from "../components/allUserLine.vue";
    import axios from "axios";

    export default {
        name: "allUserList",
        components: {allUserHeader, allUserLine},
        props:{
            adminUserSelect:{
                type:Number,
                required:true
            }
        },

        mounted: function() {
            console.log('mounted runs in  orgUserList');
//            axios.get('http://localhost:8000//layoutList')
            axios.get('http://localhost:8000/api/shan/allUsers?XDEBUG_SESSION_START=19181',
                {

                })

                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    this.users = response.data;
                    console.log('successful return from orgUsers');
                })
                .catch(e => {
                    console.log(e);
                    this.errors.push(e);
                });
        },
        data(){
            return{
                users:[],
                NEW_USER:1,
                SELECT_USER:0
            }
        },
        methods:{
            userSelected(msg){
                this.$emit('userSelected',[msg]);
            }
        }


    }
</script>

<style scoped>
    .allUserContainer {
        width: 100%;
    }

</style>

