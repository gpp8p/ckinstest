<template>
    <span class="lline">
        <span class="gitem">Name</span>
        <span class="gitem">Email</span>
        <span class="gitem"><a href="#" class="gitem" v-on:click="findUser">Find:</a><input ref="email" type="text" size="32" maxlength="50" v-model="email" /></span>
    </span>

</template>

<script>
    import axios from "axios";

    export default {
        name: "allUserHeader",
        data() {
            return {
                email: '',
                users: []
            }
        },
        watch:{
          users: function(){
            if(this.users.length>0){
                this.$emit('userFound', [this.users[0].name, this.users[0].email, this.users[0].id, this.users[0].is_admin]);
            }else{
                this.$emit('userNotFound');
            }
          }
        },
        methods: {
            findUser(){
                console.log('find:', this.email);
                axios.get('http://localhost:8000/api/shan/findUser?XDEBUG_SESSION_START=19181',
                    {
                        params: {
                            email:this.email
                        }
                    })
                    .then(response => {
// eslint-disable-next-line no-debugger
                        // JSON responses are automatically parsed.
                        this.users = response.data;
                        console.log('successful return from findUser', this.users);
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
    .lline {
        display: grid;
        grid-template-columns: 30% 10% 60%;
        font-size: medium;
        font-family: Arial;
        align-items: left;
        background-color: darkgreen;
        color: white;
    }
    .gitem {
        text-align: left;
        margin-left: 5px;
        color: white;
    }
</style>
