<template>
    <span>
        <span v-if="this.logStatus==this.NOT_LOGGED">
            <span class="welcomeMessage">Welcome Guest ! <a @click="showLogin">Please Log In</a></span>
        </span>
        <span v-if="this.logStatus==this.SHOW_LOGIN">
            <label class="labelStyle" for="email">Email:</label>
            <input type="text" id="email" name="email" size="25" v-model="email" />
            <label class="labelStyle" for="password">Password:</label>
            <input type="password" id="password" size="30" maxlength="65" v-model="password"/>
            <span class="labelStyle">
                 <a @click="doLogin">Login </a>
                 <a class="cancel" @click="cancelLogin"> Cancel</a>
            </span>
        </span>
        <span v-if="this.logStatus==this.LOGGED_IN">
            <span class="welcomeMessage">Welcome Guest ! <a @click="doLogout">Log Out</a></span>
        </span>
    </span>
</template>

<script>
    import axios from "axios";
    export default {
        name: "loginComponent",
        data(){
            return {
                logStatus: 0,
                NOT_LOGGED:0,
                SHOW_LOGIN:1,
                LOGGED_IN:2,
                email:'',
                password:'',
                auth_token:''
            }
        },
        watch:{
            auth_token: function(){
//                debugger;
                axios.defaults.headers.common['Authorization'] = `Bearer ${this.auth_token}`;
                this.logStatus = this.LOGGED_IN;
            }
        },
        methods:{
            showLogin(){
                this.logStatus=this.SHOW_LOGIN;
            },
            cancelLogin(){
                this.logStatus=this.NOT_LOGGED;
            },
            doLogout(){

            },
            doLogin(){
                this.sendLogin(this.email, this.password);
                this.action=this.SHOW_PROFILE;
//                axios.defaults.headers.common['Authorization'] = `Bearer ${this.auth_token}`
            },
            sendLogin(email, password){
                axios.post('http://localhost:8000/api/auth/login', {
                    email: email,
                    password: password
                }).then(response=>
                {
                    console.log('login successful');
                    this.auth_token=response.data.access_token;
                }).catch(function(error) {
                    console.log(error);
                    return('error:');
                });
            },

        }
    }
</script>

<style scoped>
    .welcomeMessage{
        color:blue;
        font-family: Arial;
        font-size: 14px;
        padding-right: 5px;
    }
    .cancel{
        color:red;
        font-family: Arial;
        font-size: 14px;

    }
    .labelStyle{
        color:blue;
        font-family: Arial;
        font-size: 14px;
        padding: 5px;
    }
    a {
        text-decoration: underline;
    }
    a:link {
        text-decoration: underline;
        color:blue;
        font-family: Arial;
        font-size: 14px;
    }
    a:hover {
        text-decoration: underline;
        color:red;
    }

</style>
