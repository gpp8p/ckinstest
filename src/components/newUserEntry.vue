<template>
    <div v-if="adminUserSelect==this.NEW_USER">
        <span class="newUserBody">
            <span class="inputPrompt">User Name:</span><span><input ref="name" type="text" size="40" v-model="name" @keydown.tab.exact = "nameCheck" required = true /></span>
            <span class="inputPrompt">User Email:</span><span><input ref="email" type="text" size="50" v-model="name" @keydown.tab.exact = "emailCheck" required = true /></span>
            <span class="inputPrompt">User Password:</span><span><input ref="email" type="text" size="40" v-model="name" @keydown.tab.exact = "pwCheck" required = true /></span>
            <span class="inputPrompt">Password Repeat:</span><span><input ref="email" type="text" size="40" v-model="name" @keydown.tab.exact = "pwrCheck" required = true /></span>
        </span>
        <span class="newUserFooter">
            <a href="#" class="linkStyle" v-on:click="newUserSave" >Create This User</a>
        </span>
    </div>
</template>

<script>
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
                SELECT_USER:0
            }
        },
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

</style>
