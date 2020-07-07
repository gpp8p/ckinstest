<template>
        <div v-if="adminUserSelect==this.SELECT_USER" >
            <allUserHeader @userFound="userFound" @userNotFound="userNotFound"></allUserHeader>
            <div class="allUserContainer">
                <allUserLine v-for="(user, index) in this.users"
                             :key="index"
                             :id="user.id"
                             :name="user.name"
                             :email="user.email"
                             :selectedId="selectedId"
                             @userSelected="userSelected"
                             @userUnSelected="userUnSelected"
                >
                </allUserLine>
            </div>

    </div>
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
            },
            allUserRefresh:{
                type: Number,
                required: true
            },
            selectedId:{
                type: Number,
                required: true
            }
        },

        mounted: function() {
            console.log('mounted runs in  orgUserList');
//            axios.get('http://localhost:8000//layoutList')
            this.getAllUsers();

        },
        watch:{
            allUserRefresh: function(){
                this.getAllUsers();
            }
        },
        data(){
            return{
                users:[],
                NEW_USER:1,
                SELECT_USER:0
            }
        },
        methods:{
            userFound(msg){
              console.log(msg);
              this.$emit('userFound', msg);
            },
            userNotFound(){
              alert('this user not found');
            },
            userSelected(msg){
                this.$emit('userSelected',[msg]);
            },
            userUnSelected(msg){
//                debugger;
                this.$emit('userUnSelected',[msg]);
            },
            getAllUsers(){
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
            }
        }


    }
</script>

<style scoped>
    .allUserContainer {
        width: 100%;
        height:135px;
        overflow: auto;
    }

</style>

