<template>
    <div v-if="adminUserSelect==this.NEW_USER">
        <span class="newUserBody">
            <span class="inputPrompt">User Name:</span><span><input ref="name" type="text" size="40" v-model="name" @keydown.tab.exact = "nameCheck" required = true /></span>
            <span class="inputPrompt">User Email:</span><span><input ref="email" type="text" size="50" v-model="email" @keydown.tab.exact = "emailCheck" required = true /></span>
            <span class="inputPrompt">User Password:</span><span><input ref="pw1" type="text" size="40" v-model="password" @keydown.tab.exact = "pwCheck" required = true /></span>
            <span class="inputPrompt">Password Repeat:</span><span><input ref="pw2" type="text" size="40" v-model="password2" @keydown.tab.exact = "pwrCheck" required = true /></span>
        </span>
        <span class="newUserFooter">
            <a href="#" class="linkStyle" v-on:click="newUserSave" >Create This User</a>
        </span>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "newUserEntry",
        props:{
            adminUserSelect:{
                type:Number,
                required:true
            }
        },
        data(){
            return{
                NEW_USER:1,
                SELECT_USER:0,
                name: '',
                email: '',
                password: '',
                password2: ''
            }
        },
        methods: {
            newUserSave(){
 //               debugger;
                if(this.password==this.password2){
                    axios.post('http://localhost:8000/api/shan/createUser?XDEBUG_SESSION_START=19311', {
                        name: this.name,
                        email: this.email,
                        password: this.password,
                    }).then(response=>
                    {
//            debugger;
                        console.log(response);
                        this.$emit('userSaved',[response.data.description, response.data.userId, response.data.userName]);
//                this.$refs.editGrid.createBlankLayout(msg[2],msg[3],msg[1],msg[0]);
                    }).catch(function(error) {
                        console.log(error);
                    });

                }else{
                    alert('Passwords must match!');
                }
            }
        }
    }
</script>

<style scoped>
    .newUserBody {
        height: 72%;
        margin-left: 10px;
        margin-right: 10px;
        display: grid;
        grid-template-columns: 30% 70%;
        grid-template-rows: 15% 15% 15% 15%
    }
    .newUserFooter {
        height: 10%;
        margin-left: 10px;
        margin-right: 10px;
    }
    .linkStyle {
        margin-left: 10px;
        margin-right: 10px;
        color: blue;
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;
    }
    .inputPrompt {
        margin-left: 10px;
        margin-right: 10px;
        color: black;
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;
    }

</style>
