<template>
    <span>
        <span v-if="this.logStatus==this.NOT_LOGGED">
            <span class="welcomeMessage">Welcome {{this.loggedInUser}} ! <a @click="showLogin">Please Log In</a></span>
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
            <span class="welcomeMessage">Welcome {{this.loggedInUser}} ! <a @click="doLogout">Log Out</a></span>
        </span>
    </span>
</template>

<script>
    import axios from "axios";
    export default {
        name: "loginComponent",
        mounted(){
//            debugger;
            console.log('login component mounted',this.credentials);
            if(this.credentials.bearerToken.length>0){
                console.log('setting token from session data');
                axios.defaults.headers.common['Authorization'] = `Bearer ${this.credentials.bearerToken}`;
                this.loggedInUser= this.credentials.loggedInUser;
                this.is_admin = this.credentials.is_admin;
                this.user_id = this.credentials.loggedInUserId;
            }else{
                this.sendLogin('GuestUser@nomail.com', 'GuestUser');
            }
/*
            axios.post('http://localhost:8000/api/auth/loggedInUser?XDEBUG_SESSION_START=15022', {
            }).then(response=>
            {
                console.log('login successful');
                this.auth_token=response.data.access_token;
                this.loggedInUser = response.data.userName;
                if(this.loggedInUser=='Guest') this.logStatus=this.NOT_LOGGED;
            }).catch(function(error) {
                console.log(error);
                return('error:');
            });

 */
        },
        props:{
          credentials:{
              type: Object,
              required: true
          }
        },
        data(){
            return {
                logStatus: 0,
                NOT_LOGGED:0,
                SHOW_LOGIN:1,
                LOGGED_IN:2,
                email:'',
                password:'',
                auth_token:'',
                loggedInUser:'',
                is_admin:0,
                user_id:0,
            }
        },
        watch:{
            auth_token: function(){
                console.log('auth_token installed');
                debugger;
                axios.defaults.headers.common['Authorization'] = `Bearer ${this.auth_token}`;
                this.logStatus = this.LOGGED_IN;
                this.$emit('tokenInstalled', [this.auth_token, this.loggedInUser, this.is_admin, this.user_id]);
            },
            loggedInUser: function(){
                if(this.loggedInUser=='GuestUser'){
                    this.logStatus=this.NOT_LOGGED;
                }else{
                    this.logStatus=this.LOGGED_IN;
                }
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
                sessionStorage.clear();
                this.sendLogin('GuestUser@nomail.com', 'GuestUser');
            },
            doLogin(){
                this.sendLogin(this.email, this.password);
                this.action=this.SHOW_PROFILE;
//                axios.defaults.headers.common['Authorization'] = `Bearer ${this.auth_token}`
            },
            sendLogin(email, password){
                axios.post('http://localhost:8000/api/auth/login?XDEBUG_SESSION_START=15022', {
                    email: email,
                    password: password
                }).then(response=>
                {
                    console.log('login successful');
                    console.log(response.data);
                    this.auth_token=response.data.access_token;
                    this.loggedInUser = response.data.userName;
                    this.is_admin = response.data.is_admin;
                    this.user_id = response.data.userId;
                    this.$emit('userLogged',[this.auth_token, this.loggedInUser, this.is_admin]);
                }).catch(function(error) {
                    console.log(error);
                    return('error:');
                });
            },
            setCookie(token){
                axios.post('http://localhost:8000/api/shan/setCookie?XDEBUG_SESSION_START=15022', {
                    token: token
                }).then(response=>
                {
                    console.log('cookie set',response);
                }).catch(function(error) {
                    console.log(error);
                    return('error:');
                });
            }

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
